# キーマップ編集

roBaキーボードのキーマップ (`config/roBa.keymap`) を編集するスキル。

## 編集前の確認事項

1. `config/roBa.keymap` を読み込み、現在のレイヤー構成を把握する
2. 変更内容をユーザーに説明し、承認を得てからファイルを編集する
3. 変更後はキーマップの整合性を確認する

## roBaのキーマトリクス構成

- 11列 x 4行、全42キーポジション（左手6列 + 右手5列）
- 右手は `col-offset: 6` でオフセットされている
- 左手にロータリーエンコーダ（EC11）、右手にトラックボール（PMW3610）

### キーポジション番号マッピング（0始まり）

```
 0  1  2  3  4                    5  6  7  8  9
10 11 12 13 14 15            16  17 18 19 20 21
22 23 24 25 26 27            28  29 30 31 32 33
34 35 36 37 38 39            40  41                42
```

- 0-4: 左手上段（Q W E R T）
- 5-9: 右手上段（Y U I O P）
- 10-15: 左手中段（A S D F G + 左親指外側キー）
- 16-21: 右手中段（- H J K L '）
- 22-27: 左手下段（Z X C V B + 左親指外側キー2）
- 28-33: 右手下段（; N M , . /）
- 34-39: 左手最下段（Ctrl Win Alt 変換/Layer Space 無変換/Layer）
- 40-42: 右手最下段（BS Enter DEL）

## 現在のレイヤー構成（7レイヤー）

| # | 名前 | 用途 |
|---|------|------|
| 0 | default_layer | QWERTY配列のベースレイヤー |
| 1 | FUNCTION | F1-F12キー |
| 2 | NUM | 数字・記号 |
| 3 | ARROW | 矢印キー・ナビゲーション |
| 4 | MOUSE | マウスボタン（automouse-layer） |
| 5 | SCROLL | スクロールモード（scroll-layers） |
| 6 | layer_6 | Bluetooth設定・ブートローダー |

## bindings配列のフォーマットルール

各レイヤーの `bindings` は必ず42エントリ（42キーポジション分）必要。行ごとの区切りは:

```
bindings = <
    // 左手上段5キー                                右手上段5キー
    &kp Q  &kp W  &kp E  &kp R  &kp T              &kp Y  &kp U  &kp I  &kp O  &kp P
    // 左手中段5キー + 左親指外側1キー    右親指外側1キー + 右手中段5キー
    &kp A  &kp S  &kp D  &kp F  &kp G  &kp X       &kp X  &kp H  &kp J  &kp K  &kp L  &kp SQT
    // 左手下段5キー + 左親指外側1キー    右親指外側1キー + 右手下段5キー
    &kp Z  &kp X  &kp C  &kp V  &kp B  &kp X       &kp X  &kp N  &kp M  &kp COMMA  &kp DOT  &kp SLASH
    // 左最下段6キー（Ctrl Win Alt 変換 Space 無変換）   右最下段3キー（BS Enter DEL）
    &kp X  &kp X  &kp X  &kp X  &kp X  &kp X       &kp X  &kp X                              &kp X
>;
```

## DTS構文の注意点

- ビヘイビアは `&` プレフィックス付き（`&kp`, `&mo`, `&lt`, `&trans` 等）
- `bindings = < ... >;` の中はカンマなしのスペース区切り
- 修飾子は関数形式: `LS()`, `LC()`, `LA()`, `LG()` でネスト可能（例: `&kp LS(LG(S))`）
- `&trans` は透過（下位レイヤーに委譲）、`&none` はブロック

## コンボの編集

```dts
combos {
    compatible = "zmk,combos";
    combo_name {
        bindings = <&kp KEY>;
        key-positions = <POS1 POS2>;  // 上記キーポジション番号を参照
        timeout-ms = <50>;            // オプション（デフォルト50ms）
        layers = <0 1>;              // オプション（有効レイヤー制限）
    };
};
```

## カスタムビヘイビアの追加

### Hold-Tap
```dts
behaviors {
    my_ht: my_hold_tap {
        compatible = "zmk,behavior-hold-tap";
        #binding-cells = <2>;
        tapping-term-ms = <200>;
        flavor = "balanced";
        bindings = <&kp>, <&kp>;
    };
};
```

### マクロ
```dts
macros {
    my_macro: my_macro {
        compatible = "zmk,behavior-macro";
        #binding-cells = <0>;
        wait-ms = <30>;
        tap-ms = <40>;
        bindings = <&kp H &kp I>;
    };
};
```

## エンコーダバインディング

各レイヤーで `sensor-bindings` を設定可能:
```dts
sensor-bindings = <&inc_dec_kp PG_UP PAGE_DOWN>;
```

## 変更後の確認チェックリスト

- [ ] 各レイヤーの `bindings` が42エントリであること
- [ ] `&trans` / `&none` の使い分けが正しいこと
- [ ] コンボの `key-positions` が正しいキーポジション番号を参照していること
- [ ] カスタमビヘイビア/マクロの `#binding-cells` が正しいこと
- [ ] Layer 4 (MOUSE) と Layer 5 (SCROLL) はトラックボール用なので変更時は注意
- [ ] Bluetooth操作（Layer 6）の `&bt BT_CLR` / `&bt BT_CLR_ALL` が誤操作しにくい位置にあること
