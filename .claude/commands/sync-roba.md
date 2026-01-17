# Sync from zmk-config-roBa

zmk-config-roBa リポジトリからキーバインディングと設定を同期します。

## 同期元
https://github.com/shomatan/zmk-config-roBa

## 手順

1. roBa のキーマップファイルを取得して内容を確認:
   - `https://raw.githubusercontent.com/shomatan/zmk-config-roBa/main/config/roBa.keymap`

2. glove43tb のキーマップに適用:
   - ファイル: `config/glove43tb.keymap`
   - 維持するもの:
     - `#include <behaviors/rgbled_widget.dtsi>` (LED widget)
     - `#define ZMK_MOUSE_DEFAULT_SCRL_VAL` の値
   - 同期するもの:
     - レイヤー構成とキーバインディング
     - behaviors（scroll_up_down, hold_layer_1 など）
     - macros（KVM など）
     - combos
   - 適用しないもの:
     - `&trackball { automouse-layer; scroll-layers; }` (badjeff ドライバー非互換)

3. roBa の設定ファイルを確認:
   - `https://raw.githubusercontent.com/shomatan/zmk-config-roBa/main/boards/shields/roBa/roBa_R.conf`

4. glove43tb の設定に適用可能な項目を確認:
   - ファイル: `boards/shields/glove43tb/glove43tb_R.conf`
   - 注意: PMW3610 の Kconfig 設定は異なるドライバーのため非互換
     - badjeff ドライバー: デバイスツリーで設定 (`glove43tb_R.overlay`)
     - kumamuk-git ドライバー: Kconfig で設定 (`CONFIG_PMW3610_*`)

5. automouse/scroll-layers の設定を確認:
   - ファイル: `boards/shields/glove43tb/glove43tb_R.overlay`
   - `zip_temp_layer` のタイムアウト値を roBa に合わせる

## ドライバーの違い

| 項目 | glove43tb (badjeff) | roBa (kumamuk-git) |
|------|---------------------|-------------------|
| CPI設定 | overlay: `cpi = <400>;` | conf: `CONFIG_PMW3610_CPI=400` |
| 向き | overlay: `invert-x;` `invert-y;` | conf: `CONFIG_PMW3610_ORIENTATION_180=y` |
| automouse | overlay: `zip_temp_layer` | keymap: `automouse-layer` |
| scroll-layers | overlay: `scroller { layers = <5>; }` | keymap: `scroll-layers` |

## 変更後

変更をコミットしてプッシュしてください。コミットメッセージ例:
```
feat: sync keybindings from zmk-config-roBa

- Update keymap with roBa-style layer configuration
- Sync automouse timeout setting

Reference: https://github.com/shomatan/zmk-config-roBa
```
