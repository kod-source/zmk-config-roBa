# ZMKファームウェアビルド

roBaキーボードのファームウェアをビルド・書き込みする手順。

## ビルド方法

### GitHub Actions（標準）

pushまたはPRで自動ビルドが実行される。手動実行も可（workflow_dispatch）。

1. 変更をコミット・プッシュする
2. GitHub Actions の Artifacts から UF2 ファイルをダウンロード
3. ビルド成果物:
   - `roBa_R-seeeduino_xiao_ble-zmk.uf2` — 右手（セントラル）
   - `roBa_L-seeeduino_xiao_ble-zmk.uf2` — 左手（ペリフェラル）
   - `settings_reset-seeeduino_xiao_ble-zmk.uf2` — 設定リセット用

### ローカルビルド（zmk-workspace使用）

[zmk-workspace](https://github.com/kot149/zmk-workspace) を使うとローカルで高速ビルドできる（約15秒）。

```bash
# セットアップ（初回のみ）
git clone https://github.com/kot149/zmk-workspace.git
cd zmk-workspace
nix develop  # または Dev Container

# zmk-configをconfig/にclone
cd config
git clone <your-zmk-config-repo>
cd ..

# 初期化（west.ymlの外部モジュールも自動解決）
just init config/zmk-config-roBa

# ビルド
just build roBa_R   # 右手
just build roBa_L   # 左手

# 書き込み（Nix + macOS/Windowsのみ）
just flash roBa_R
just flash roBa_R -r  # 再ビルド+書き込み
```

## ファームウェア書き込み手順

1. USBでマイコンをPCに接続
2. リセットボタンを**2回**押す → 「XIAO SENSE」ドライブが出現（ブートローダーモード）
3. UF2ファイルをドラッグ&ドロップ
4. macOSではエラーが出るが無視してOK

### 書き込み順序（初回 or ペアリング問題時）

1. 左手: `settings_reset` → `roBa_L`
2. 右手: `settings_reset` → `roBa_R`
3. 両方の電源ON → リセットボタン1回 → Bluetooth「roBa」でペアリング

### キーマップのみ変更した場合

- **右手（セントラル）のみ書き換えればOK**
- `settings_reset` は不要
- PC側のペアリング情報は削除して再接続が推奨

## build.yaml（ビルドマトリクス）

```yaml
include:
  - board: seeeduino_xiao_ble
    shield: roBa_R
    snippet: studio-rpc-usb-uart  # ZMK Studio用
  - board: seeeduino_xiao_ble
    shield: roBa_L
  - board: seeeduino_xiao_ble
    shield: settings_reset
```

## キーマップ描画（SVG生成）

```bash
# GitHub Actions で手動実行
# .github/workflows/draw.yml → workflow_dispatch
# 結果: keymap-drawer/roBa.svg
```
