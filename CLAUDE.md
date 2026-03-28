# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

roBa キーボードの ZMK ファームウェア設定リポジトリ。左右分割キーボードで、右手側にトラックボール（PMW3610）、左手側にロータリーエンコーダ（EC11）を搭載。MCU は Seeeduino XIAO BLE（nRF52840）。

## Build

ローカルビルド環境はなく、GitHub Actions でビルドする。push / PR / workflow_dispatch で自動実行される。

- ビルドワークフロー: `.github/workflows/build.yml` → `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3-branch` を呼び出し
- キーマップ描画: `.github/workflows/draw.yml` → workflow_dispatch で手動実行（`keymap-drawer/roBa.svg` を生成）

ビルド成果物（UF2ファイル）は GitHub Actions の Artifacts からダウンロードする。

## Architecture

### Split 構成

- **右手（roBa_R）**: セントラル（`ZMK_SPLIT_ROLE_CENTRAL`）。トラックボール（PMW3610、SPI接続）搭載。ZMK Studio 有効
- **左手（roBa_L）**: ペリフェラル。ロータリーエンコーダ（EC11）搭載

### Key Files

- `config/roBa.keymap` — キーマップ定義（7レイヤー: default, FUNCTION, NUM, ARROW, MOUSE, SCROLL, BT/設定）
- `boards/shields/roBa/roBa.dtsi` — 共通ハードウェア定義（キーマトリクス、物理レイアウト、トランスフォーム）
- `boards/shields/roBa/roBa_R.overlay` — 右手固有設定（トラックボール、SPI、ピン設定）
- `boards/shields/roBa/roBa_L.overlay` — 左手固有設定（エンコーダ、キーマトリクス列）
- `boards/shields/roBa/roBa_R.conf` — 右手 Kconfig（PMW3610設定、ZMK Studio）
- `boards/shields/roBa/roBa_L.conf` — 左手 Kconfig（バッテリーレポート、エンコーダ）
- `config/west.yml` — West マニフェスト（ZMK v0.3-branch + kumamuk-git/zmk-pmw3610-driver）
- `build.yaml` — GitHub Actions ビルドマトリクス

### Dependencies

- ZMK: `zmkfirmware/zmk` v0.3-branch
- トラックボールドライバ: `kumamuk-git/zmk-pmw3610-driver` main branch

### Matrix Layout

11列 x 4行。左手6列 + 右手5列（`col-offset: 6`）。コンボ、マクロ、カスタム hold-tap behavior を使用。

## Keymap Editing Rules

キーマップ (`config/roBa.keymap`) を編集する際のルール:

- 編集前に必ず現在のキーマップを読み、レイヤー構成・コンボ・マクロを把握する
- 変更内容をユーザーに説明し承認を得てから編集する
- 各レイヤーの `bindings` は必ず **42エントリ**（42キーポジション分）にする
- Layer 4 (MOUSE) と Layer 5 (SCROLL) はトラックボール自動切替用。変更時は `&trackball` の `automouse-layer` / `scroll-layers` との整合性を確認
- キーマップのみの変更は右手（セントラル）ファームウェアの書き換えだけでOK
- 詳細なリファレンスは `.claude/skills/` 配下のスキルを参照（`edit-keymap`, `zmk-reference`, `zmk-build`）
