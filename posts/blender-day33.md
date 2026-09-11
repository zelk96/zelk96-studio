---
layout: default
title: "Blender学習33日目｜DAZのストランドヘアをBlenderで正常に表示する"
permalink: /posts/blender-day33.html
date: '2026-09-11'
display_date: 2026.09.11
learning_log: true
day: 33
---

# Blender学習33日目｜DAZのストランドヘアをBlenderで正常に表示する

今日は、DAZ Studioから読み込んだモデルの髪と白目の問題を修正した。

FBXで読み込んだ髪は白い塊になり、本来の形状や色が再現されていなかった。

![FBXで崩れた髪](/zelk96-studio/images/day33/01.png)

## 原因

使用している髪は、通常のポリゴンではなくdForceのストランドベースヘアだった。

FBXでは頂点こそ出力されるものの、ポリゴン数が0になっていた。OBJも試したが、レンダリング可能な髪としては読み込めなかった。

## Diffeomorphicで再インポート

BlenderへDiffeomorphic DAZ Importerを導入。DAZ Studio側でも専用スクリプトを設定し、シーンと同じ場所へDBZファイルを書き出した。

Blenderの`Easy Import DAZ`からDUFとDBZを使って読み込むことで、モデル・衣装・瞳・髪の形状とマテリアルを正常に取得できた。

ただし、この段階の髪は線の状態だったため、マテリアルプレビューでは見えてもレンダリングすると消えてしまった。

![レンダリングで消えた髪](/zelk96-studio/images/day33/02.png)

## ヘアーカーブへ変換

Diffeomorphicの`Make Hair`を使い、髪をレンダリング可能なヘアーカーブへ変換した。

- Strand Type：Line
- Output：Hair Curves
- Render Children：0
- Keep Material：オン
- Parent To Head：オン

これで髪の形状と色を保ったまま、Cyclesでも正常にレンダリングできるようになった。

## 衣装のめり込み修正

パンツの中央に穴が開いているように見えたが、透過設定ではなく、身体が衣装を突き抜けていたことが原因だった。

編集モードでパンツ全体を選択し、`Alt + S`で法線方向へ約1mm膨らませて修正した。

![髪と衣装の修正完了](/zelk96-studio/images/day33/03.png)

## 今回の結果

- ストランドベースヘアの形状と色を復元
- 白目の問題を解消
- 髪をCyclesでレンダリング可能に変換
- パンツのめり込みを修正
- 正常版モデルを`Jiuyin_Fixed.blend`として保存

次は自作した帽子を新しいモデルへ移植し、監獄シーンへ配置する。
