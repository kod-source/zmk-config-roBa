# ZMKリファレンス

ZMK firmwareのキーマップ設定に関する包括的リファレンス。

## キーコード一覧

### アルファベット・数字
`&kp A` ~ `&kp Z`, `&kp N1` ~ `&kp N0` (NUMBER_1 ~ NUMBER_0)

### 修飾キー
| キー | エイリアス |
|------|-----------|
| `LEFT_SHIFT` | `LSHIFT`, `LSHFT` |
| `RIGHT_SHIFT` | `RSHIFT`, `RSHFT` |
| `LEFT_CONTROL` | `LCTRL` |
| `RIGHT_CONTROL` | `RCTRL` |
| `LEFT_ALT` | `LALT` |
| `RIGHT_ALT` | `RALT` |
| `LEFT_GUI` | `LGUI`, `LEFT_WIN`, `LEFT_COMMAND`, `LCMD` |
| `RIGHT_GUI` | `RGUI`, `RIGHT_WIN`, `RIGHT_COMMAND`, `RCMD` |

### 修飾子ラッパー関数
`LS()`, `RS()`, `LC()`, `RC()`, `LA()`, `RA()`, `LG()`, `RG()` — ネスト可能

```dts
&kp LC(C)       // Ctrl+C
&kp LS(LG(S))   // Shift+Cmd+S
&kp LA(F4)      // Alt+F4
```

### 記号
| キーコード | 記号 | キーコード | 記号 |
|-----------|------|-----------|------|
| `EXCLAMATION` / `EXCL` | ! | `AT_SIGN` / `AT` | @ |
| `HASH` / `POUND` | # | `DOLLAR` / `DLLR` | $ |
| `PERCENT` / `PRCNT` | % | `CARET` | ^ |
| `AMPERSAND` / `AMPS` | & | `ASTERISK` / `ASTRK` | * |
| `LEFT_PARENTHESIS` / `LPAR` | ( | `RIGHT_PARENTHESIS` / `RPAR` | ) |
| `LEFT_BRACKET` / `LBKT` | [ | `RIGHT_BRACKET` / `RBKT` | ] |
| `LEFT_BRACE` / `LBRC` | { | `RIGHT_BRACE` / `RBRC` | } |
| `MINUS` | - | `UNDERSCORE` / `UNDER` | _ |
| `EQUAL` | = | `PLUS` | + |
| `BACKSLASH` / `BSLH` | \ | `PIPE` | \| |
| `SEMICOLON` / `SEMI` | ; | `COLON` | : |
| `SINGLE_QUOTE` / `SQT` | ' | `DOUBLE_QUOTES` / `DQT` | " |
| `COMMA` | , | `PERIOD` / `DOT` | . |
| `SLASH` / `FSLH` | / | `QUESTION` / `QMARK` | ? |
| `GRAVE` | ` | `TILDE` | ~ |

### 制御キー
`ESCAPE` / `ESC`, `RETURN` / `RET` / `ENTER`, `SPACE`, `TAB`, `BACKSPACE` / `BSPC`, `DELETE` / `DEL`, `INSERT` / `INS`

### ナビゲーション
`HOME`, `END`, `PAGE_UP` / `PG_UP`, `PAGE_DOWN` / `PG_DN`, `UP_ARROW` / `UP`, `DOWN_ARROW` / `DOWN`, `LEFT_ARROW` / `LEFT`, `RIGHT_ARROW` / `RIGHT`

### ファンクションキー
`F1` ~ `F24`

### テンキー
`KP_NUMBER_1` ~ `KP_NUMBER_0` / `KP_N1` ~ `KP_N0`, `KP_PLUS`, `KP_MINUS`, `KP_MULTIPLY`, `KP_DIVIDE`, `KP_ENTER`, `KP_DOT`

### メディアキー
`C_VOLUME_UP` / `C_VOL_UP`, `C_VOLUME_DOWN` / `C_VOL_DN`, `C_MUTE`, `C_PLAY_PAUSE` / `C_PP`, `C_NEXT`, `C_PREVIOUS` / `C_PREV`, `C_BRIGHTNESS_INC` / `C_BRI_UP`, `C_BRIGHTNESS_DEC` / `C_BRI_DN`, `C_POWER` / `C_PWR`, `C_SLEEP`

### 日本語IME
`INT_HENKAN` (変換), `INT_MUHENKAN` (無変換), `LANG1` (かな), `LANG2` (英数)

---

## ビヘイビア一覧

### 基本ビヘイビア

| ビヘイビア | 構文 | 説明 |
|-----------|------|------|
| Key Press | `&kp KEYCODE` | キー送信 |
| Transparent | `&trans` | 下位レイヤーに委譲 |
| None | `&none` | 何もしない（ブロック） |
| Key Toggle | `&kt KEYCODE` | キーのON/OFF切替 |
| Sticky Key | `&sk MODIFIER` | 次のキーと組み合わせて自動解除 |
| Caps Word | `&caps_word` | 自動解除CapsLock |
| Key Repeat | `&key_repeat` | 直前のキーを再送 |
| Grave Escape | `&gresc` | 単体=ESC、Shift/GUI時=` |

### レイヤー操作

| ビヘイビア | 構文 | 説明 |
|-----------|------|------|
| Momentary | `&mo LAYER` | 押している間だけ有効 |
| Layer-Tap | `&lt LAYER KEYCODE` | 長押し=レイヤー、タップ=キー |
| To Layer | `&to LAYER` | レイヤー切替 |
| Toggle Layer | `&tog LAYER` | ON/OFF切替 |
| Sticky Layer | `&sl LAYER` | 次の1キーのみ有効 |

### Hold-Tap

| ビヘイビア | 構文 | 説明 |
|-----------|------|------|
| Mod-Tap | `&mt MODIFIER KEYCODE` | 長押し=修飾キー、タップ=キー |
| Layer-Tap | `&lt LAYER KEYCODE` | 長押し=レイヤー、タップ=キー |
| カスタム | ユーザー定義 | `zmk,behavior-hold-tap` で自由に定義 |

### Hold-Tap フレーバー

| フレーバー | ホールド判定 |
|-----------|------------|
| `hold-preferred` | tapping-term経過 **または** 他キー押下 |
| `balanced` | tapping-term経過 **または** 他キーが押されてリリース |
| `tap-preferred` | tapping-term経過のみ |
| `tap-unless-interrupted` | tapping-term内に他キー押下で即ホールド |

### Hold-Tap パラメータ

| パラメータ | 説明 | デフォルト |
|-----------|------|-----------|
| `tapping-term-ms` | ホールド判定時間(ms) | 200 |
| `quick-tap-ms` | 再タップ猶予（連打対応） | 無効 |
| `require-prior-idle-ms` | 高速タイピング誤発動防止 | 無効 |
| `hold-trigger-key-positions` | ホールド判定対象キー限定 | 全キー |
| `hold-trigger-on-release` | リリース時判定 | false |
| `retro-tap` | 単独リリースでタップ | false |

### Bluetooth

```dts
#include <dt-bindings/zmk/bt.h>
```

| ビヘイビア | 説明 |
|-----------|------|
| `&bt BT_SEL N` | プロファイルN選択 (0-4) |
| `&bt BT_CLR` | 現在のペアリング消去 |
| `&bt BT_CLR_ALL` | 全ペアリング消去 |
| `&bt BT_NXT` / `&bt BT_PRV` | プロファイル順送り/逆送り |
| `&bt BT_DISC N` | プロファイルN切断 |

### マウス

```dts
#include <dt-bindings/zmk/pointing.h>
// CONFIG_ZMK_POINTING=y が必要
```

| ビヘイビア | 説明 |
|-----------|------|
| `&mkp MB1` / `LCLK` | 左クリック |
| `&mkp MB2` / `RCLK` | 右クリック |
| `&mkp MB3` / `MCLK` | 中クリック |
| `&mkp MB4` / `&mkp MB5` | 戻る/進む |
| `&mmv MOVE_UP/DOWN/LEFT/RIGHT` | マウス移動 |
| `&msc SCRL_UP/DOWN/LEFT/RIGHT` | スクロール |

### 出力選択

```dts
#include <dt-bindings/zmk/outputs.h>
```

| ビヘイビア | 説明 |
|-----------|------|
| `&out OUT_USB` | USB出力 |
| `&out OUT_BLE` | BLE出力 |
| `&out OUT_TOG` | USB/BLEトグル |

### システム

| ビヘイビア | 説明 |
|-----------|------|
| `&bootloader` | ブートローダーモード |
| `&sys_reset` | システムリセット |

---

## コンボ構文

```dts
combos {
    compatible = "zmk,combos";
    combo_name {
        bindings = <&kp KEY>;
        key-positions = <POS1 POS2>;
        timeout-ms = <50>;              // オプション
        layers = <0 1>;                 // オプション
        slow-release;                   // オプション
        require-prior-idle-ms = <150>;  // オプション
    };
};
```

## マクロ構文

```dts
macros {
    my_macro: my_macro {
        compatible = "zmk,behavior-macro";
        #binding-cells = <0>;
        wait-ms = <30>;
        tap-ms = <40>;
        bindings = <&kp H &kp E &kp L &kp L &kp O>;
    };
};
```

### マクロ制御コマンド
`&macro_tap`, `&macro_press`, `&macro_release`, `&macro_wait_time MS`, `&macro_tap_time MS`, `&macro_pause_for_release`

### パラメータ付きマクロ
- 1パラメータ: `zmk,behavior-macro-one-param` + `#binding-cells = <1>`
- 2パラメータ: `zmk,behavior-macro-two-param` + `#binding-cells = <2>`
- 転送: `&macro_param_1to1`, `&macro_param_1to2`, `&macro_param_2to1`, `&macro_param_2to2`

## 条件付きレイヤー

```dts
conditional_layers {
    compatible = "zmk,conditional-layers";
    tri_layer {
        if-layers = <1 2>;
        then-layer = <3>;
    };
};
```

## エンコーダ

```dts
sensor-bindings = <&inc_dec_kp PG_UP PAGE_DOWN>;
```

## トラックボール（PMW3610）

`roBa_R.conf` で設定:
- `CONFIG_PMW3610_CPI=400` — 感度
- `CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS=700` — 自動マウスレイヤータイムアウト
- `CONFIG_PMW3610_SCROLL_TICK=16` — スクロール感度

`roBa.keymap` の `&trackball` で設定:
- `automouse-layer = <4>` — マウスレイヤー番号
- `scroll-layers = <5>` — スクロールレイヤー番号

### トラックボール方向にキー割当（roBa_R.overlay）
```dts
#include <dt-bindings/zmk/keys.h>

trackball: trackball@0 {
    // ...
    arrows {
        layers = <3>;
        bindings = <&kp RIGHT>, <&kp LEFT>, <&kp UP>, <&kp DOWN>;
        tick = <10>;
        wait-ms = <5>;
        tap-ms = <5>;
    };
};
```
