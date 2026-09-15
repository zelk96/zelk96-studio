---
layout: default
title: "Blender実制作 Day34（中編）：DAZ Runtimeで冷淡な見下ろし表情を作る"
description: "DiffeomorphicのStandard MorphsとFACSを読み込み、まぶたと眼球の動きを切り分けながら、Jiuyin Zhuの冷たい見下ろし表情を作ったDay34中編。"
permalink: /posts/blender-day34-part2-expression.html
date: "2026-09-12"
display_date: 2026.09.12
blender_note: true
---

# Blender実制作 Day34（中編）：DAZ Runtimeで冷淡な見下ろし表情を作る

Day34前編では、`Jiuyin Zhu`を監獄へ配置し、腰へ手を当てて顎を上げた女幹部の立ちポーズを作った。

中編では表情を調整する。今回の狙いは、顔全体を下へ向けるのではなく、顎を上げたまま視線だけを下げた冷淡な表情である。

## Update Active Morphsが空だった

最初にDAZ Runtimeの`Morphs`を開き、`Update Active Morphs`を押したが、`Active Morphs`には何も表示されなかった。

読み込んだキャラクターに表情モーフが準備されていなかったため、DAZ Setup側からStandard Morphsを追加することにした。

## Standard MorphsとFACSを読み込む

アーマチュアを選択し、DAZ Setupの`Morphs`から`Import Standard Morphs`を実行した。

今回有効にした項目は次のとおり。

- `Expressions`
- `FACS`
- `FACS Expressions`
- `Transfer To Face Meshes`
- `Make All Bones Posable`

![Import Standard Morphsの設定]({{ '/images/day34-expression/01.jpg' | relative_url }})

読み込み後にDAZ Runtimeへ戻って`Update Active Morphs`を押すと、表情用のスライダーが表示された。

FACS欄にも`Brow`、`Nose`、`Mouth`、`Cheek_and_Jaw`、`Eyes`などのカテゴリが追加され、顔の部位ごとに調整できるようになった。

![Standard Morphs読み込み後のDAZ Runtime]({{ '/images/day34-expression/02.jpg' | relative_url }})

## Eye Look Downでは眼球が下を向かなかった

最初は名前から判断して、`Eye Look Down Left`と`Eye Look Down Right`を上げれば視線が下を向くと考えた。

実際に数値を変えると、今回のモデルでは眼球の向きよりも上まぶたの変化が強く見えた。両方を`1.000`まで上げると、視線が下がるというより、まぶたが落ちたジト目に近い表情になった。

![Eye Look Downを強くしたときの表情]({{ '/images/day34-expression/03.jpg' | relative_url }})

これはこれで冷たい印象には使えるが、欲しかったのは眼球そのものを下へ向ける動きだった。

## 眼球ボーンの回転も試す

ポーズモードで左右の眼球ボーンを選択し、`R`、`X`、`X`でローカルX軸回転を試した。

この方法なら眼球を上下へ向けられたが、左右の目を揃える調整や、表情を再現するときの管理が少し面倒だった。また、骨の回転とDAZモーフを同時に使うと、どちらが視線を決めているのか分かりにくくなる。

そこで眼球ボーンは`Alt + R`で初期位置へ戻し、視線もDAZ Runtimeのモーフだけで作る方針にした。

## Eye Look Up-DownとSide-Sideを使う

DAZ Runtimeの`FACS → Eyes`を確認すると、左右個別の項目とは別に、両目をまとめて動かす項目があった。

- `Eye Look Up-Down`：上下の視線
- `Eye Look Side-Side`：左右の視線

`Eye Look Up-Down`をマイナス方向へ動かすと、両方の眼球が揃って下を向いた。さらに`Eye Look Side-Side`を少しマイナスへ動かし、真正面ではなく画面横へ視線を外した。

![Eye Look Up-DownとSide-Sideによる視線調整]({{ '/images/day34-expression/04.jpg' | relative_url }})

顎は上を向いたまま、目だけが下と横へ向くため、相手を冷たく見下ろしている印象になった。

## 完成した表情の設定値

最終的に使用した主な値は次のとおり。

### Active Morphs

| モーフ                |     値 |
| --------------------- | -----: |
| `Eye Look Automatic`  |  1.000 |
| `Eye Look Down Left`  |  0.350 |
| `Eye Look Down Right` |  0.350 |
| `Eye Look Side-Side`  | -0.300 |
| `Eye Look Up-Down`    | -1.000 |
| `Eye Squint Left`     |  0.150 |
| `Eye Squint Right`    |  0.150 |
| `Mouth Sticky Power`  |  1.000 |

### FACSとEye Adjustments

| カテゴリ        | モーフ                |     値 |
| --------------- | --------------------- | -----: |
| FACS / Eyes     | `Eye Look Side-Side`  | -0.300 |
| FACS / Eyes     | `Eye Look Up-Down`    | -1.000 |
| Eye Adjustments | `Eye Look Down Left`  |  0.350 |
| Eye Adjustments | `Eye Look Down Right` |  0.350 |

左右個別の`Eye Look Side-Side Left/Right`と`Eye Look Up-Down Left/Right`は`0.000`のままにした。

![ポーズと表情を組み合わせたテストレンダー]({{ '/images/day34-expression/05.jpg' | relative_url }})

## 表情モーフはポーズアセットへ保存されなかった

前編で作った`Jiuyin_FemaleExecutive_Standing_01`を適用すると、身体のポーズは元に戻せた。しかし、DAZ Runtimeで設定した表情モーフは復元されなかった。

DAZ Runtimeには`Import Expression`はあったが、今回の画面では現在の表情をそのまま保存する分かりやすい機能を見つけられなかった。

そのため、表情設定は次のMarkdownへ手動で記録した。

```text
Jiuyin_FemaleExecutive_ColdLook_01.md
```

ポーズアセットほど便利ではないが、使用したモーフ名と値が分かれば同じ表情を再現できる。今回の記事にも値を残したため、外部のメモを開けない状況でも確認できる。

## 今回の学び

- `Update Active Morphs`が空の場合は、表情モーフ自体が未導入の可能性がある
- DAZ Setupの`Import Standard Morphs`からExpressionsとFACSを追加できる
- モーフ名に`Look Down`とあっても、モデルやカテゴリによってはまぶた側の変化が強く見える
- まぶたを下げる操作と、眼球の向きを変える操作は分けて考える
- 眼球ボーンでも視線を変えられるが、DAZ Runtimeへ統一すると管理しやすい
- `Eye Look Up-Down`で上下、`Eye Look Side-Side`で左右の視線をまとめて調整できる
- 顎を上げ、視線だけを下げると見下ろす表情を作りやすい
- ボーンのポーズアセットには、DAZ Runtimeの表情モーフが含まれない場合がある
- 再現したいモーフ値は名前付きで記録しておく

## 次回

Day34後編では、完成したポーズと表情をレンダーし、暗い背景へ溶けた黒いブーツを補助光で見せる。さらにEEVEEのサンプル数と出力解像度を調整し、完成画像を書き出す。
