# zmk-config-roBa

[roBa](https://github.com/kumamuk-git/zmk-config-roBa) のキーマップ設定リポジトリ。
Keyball39 から移行したカスタムキーマップを管理しています。

## Keymap

| Layer | 名前 | 発動方法 | 用途 |
|-------|------|----------|------|
| 0 | Default | - | QWERTY配列 |
| 1 | Numbers/Symbols | Enter長押し | 数字・記号 |
| 2 | Navigation | F長押し / Space長押し | Ctrl操作・矢印・マウスボタン |
| 3 | Numpad/Mouse | P長押し | テンキー・マウス操作 |
| 4 | Mouse | 自動 (トラックボール使用時) | マウスカーソル |
| 5 | Scroll | Layer 2/3 から発動 | トラックボールスクロール |
| 6 | Bluetooth | Layer 6 キーから発動 | BT接続管理 |

## ファームウェアのビルドとフラッシュ

1. `config/roBa.keymap` を編集して push
2. GitHub Actions が自動でファームウェア (`.uf2`) をビルド
3. Actions の Artifacts から `.uf2` をダウンロード
4. roBa を USB接続 → リセットボタン2回押し → `.uf2` をドラッグ&ドロップ