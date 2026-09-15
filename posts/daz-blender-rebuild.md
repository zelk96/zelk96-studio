---
layout: default
title: "DAZ→Blender再構築：Morph・衣装・髪のトラブル解決"
description: "DAZ StudioからBlenderへモデルを再構築する中で発生したMorph、衣装リグ、Hair Curves、帽子移植のトラブルと解決方法を整理"
permalink: /posts/daz-blender-rebuild.html
date: "2026-09-14"
display_date: 2026.09.14
blender_note: true
---

# DAZ→Blender再構築で詰まったポイントまとめ：衣装Morph・Merge Rigs・Hair Curves

今日は、DAZ Studioで作り直したJiuyinをBlenderへ再取り込みし、本番用マスターを整える作業を進めた。

見た目としては単純な再インポートだったが、実際には以下のような問題が出た。

- DAZでFavorite登録した指MorphがBlenderに入っていなかった
- Police Shirtの`Shirt Open`系MorphがBlenderに出てこない
- Temptress衣装がBlender側で個別アーマチュアになっていた
- 髪が首を少し動かしただけで大きく破綻した
- 帽子を新モデルへ移植する必要があった
- 新しく追加したハイヒールも個別アーマチュアとして取り込まれた

今回は、今後また同じ問題に遭遇したときにすぐ対処できるよう、原因と解決方法をまとめておく。

---

## 1. 指MorphはDAZ側でFavorite登録してから再インポート

今回、最初にBlenderへ取り込んだ時点では、DAZ Studio側で手・指のMorphをFavorite登録していなかった。

そのため、BlenderのDiffeomorphic側に以下のようなMorphが出てこなかった。

- Hand Fist
- Hand Grasp
- Hand Relax
- Hand Spread
- Finger Bend
- Thumb Bend
- Thumb Fist
- Thumb Grasp
- Thumb In-Out

DAZ側で左右のHand関連MorphをFavorite登録し、再度Easy Importすると、Blenderの

`DAZ Runtime > Morphs > Favorites`

に正常に表示された。

### メモ

Blender側で使いたいDAZ Morphは、事前にDAZ Studio側でFavorite登録しておく。

---

## 2. 衣装が「着られない」のではなく、メッシュが非表示だった

Temptress衣装を再インポートした際、Blender上では衣装が使えないように見えた。

しかしOutlinerを展開すると、

`Temptress Bra`
`Temptress Gloves`
`Temptress Boots`

などはアーマチュアとして取り込まれており、その内部に実際のMeshが存在していた。

Meshを表示すると、衣装自体は正常に表示できた。

つまり、

「衣装がインポートできていない」

のではなく、

「衣装Meshが非表示になっていた」

だけだった。

---

## 3. 衣装ごとのアーマチュアはMerge RigsでJiuyinへ統合

Temptress系やハイヒールは、そのまま取り込むと衣装ごとに専用アーマチュアが作られた。

旧アニメーションファイルでは、

`Jiuyin Zhu`

- `Temptress Bra Mesh`
- `Temptress Gloves Mesh`
- `Temptress Boots Mesh`

のように、衣装MeshがJiuyin本体のアーマチュアへ直接統合されていた。

新しいファイルでも同じ構造にするため、Diffeomorphicの

`DAZ Setup > Corrections > Merge Rigs`

を使用。

### Merge Rigs時の基本設定

- Only Selected Rigs：ON
- Include Hidden Rigs：OFF
- Convert to Widgets：ON
- Tie Rigs Instead：OFF

Jiuyin本体をアクティブにし、衣装アーマチュアを選択してMerge Rigsを実行する。

これにより、衣装専用アーマチュアが消え、衣装MeshがJiuyin本体へ統合された。

### 今回Mergeしたもの

- Temptress Bra
- Temptress Gloves
- Temptress Panties
- Temptress Shrug
- Temptress Boots
- High Heels

Merge後はOutlinerもかなり整理された。
ハイヒールも同じ方法で統合できた。

---

## 4. 髪が首を少し動かしただけで破綻する場合はMake Hairを忘れていないか確認

今回かなりハマったポイント。

元のHair Meshをそのまま使っている状態で`neck1`を10度程度回転させると、髪が大きく引き伸ばされて破綻した。

最初はウェイトやArmature Modifierの問題を疑ったが、旧アニメーションファイルを確認すると、

`Hair FE Side Part Straight Hair Mesh`

というHair Curvesオブジェクトが存在していた。

つまり、以前はDiffeomorphicの`Make Hair`を使ってHair MeshをHair Curvesへ変換していた。

### Make Hairの設定

`DAZ Setup > Hair > Make Hair`

- Strand Type：Line
- Output：Hair Curves
- Keep Material：ON
- Render Children：0
- Parent To Head：ON
- Single Output：ON

Make Hair後に生成されたHair Curvesでは、首を回転させても正常に追従した。

### 結論

髪が異常変形した場合、まず

`Make Hairを実行済みか`

を確認する。

---

## 5. 帽子は単体blendへ分離して再利用可能にした

以前作成した帽子は、

`Jiuyin_Hat_Final.blend`

の中にJiuyin本体や衣装ごと保存されていた。

今後使い回しやすくするため、

`Jiuyin_Hat_Asset.blend`

として帽子だけを独立させた。

帽子のControllerはGenesis9の`head`ボーンへ親子付けされていたため、

`Alt + P > トランスフォームを維持してクリア`

で親子関係を解除。

その後、Jiuyin本体や衣装を削除し、

- Hat Controller
- Hat Mesh
- Hat各パーツ

だけを残した。

新しいJiuyin側では、この帽子コレクションをAppendして、再び`head`ボーンへ親子付けする。

これで帽子を独立アセットとして再利用できるようになった。

---

## 6. 帽子から髪が飛び出す場合はHair Meshを加工してからMake Hair

帽子をそのまま被せると、頭頂部の髪が帽子を突き抜ける。

対策として、

1. 元Hair Meshを複製してBackup
2. 帽子から飛び出すHair Meshの頂点を削除
3. 加工後のHair MeshからMake Hair
4. 元Hair Meshは非表示

という流れで対応。

Hair Curves化した後ではなく、元Hair Mesh側を加工してからMake Hairする。

完全に全方向から破綻しない形にするのは難しいため、頭頂部が映るカメラでは帽子を非表示にする運用も考える。

---

## 7. Police ShirtのShirt Open系MorphがBlenderに来ない問題

今回一番原因が分かりにくかった問題。

DAZ Studio側では、

- Shirt Open
- Shirt Open L
- Shirt Open R
- Shirt Collar

をFavorite登録していた。

Easy Import側でも`DAZ Favorites`はON。

それでもBlender側にはMorphが表示されなかった。

Import時のエラーログを見ると、Diffeomorphicが衣装名を使って生成するUIクラス名が長すぎて、Blender側の文字数制限に引っかかっていた。

元の衣装名は、

`BS Professional Uniform Shirt for Genesis 9`

のように非常に長かった。

そこでDAZ Studio側のNode名を短く変更。

### 変更例

`BS Professional Uniform Shirt for Genesis 9`
→ `PoliceShirt`

`BS Professional Uniform Office Shirt for Genesis 9`
→ `OfficeShirt`

`BS Professional Uniform Skirt for Genesis 9`
→ `UniformSkirt`

`AP Stylish Mini High Heels G9`
→ `HighHeels`

髪も、

`FE Side Part Straight Hair`
→ `JiuyinHair`

へ短縮。

その状態で再エクスポート・Easy Importすると、

Import時のエラーが消え、Police Shirt Mesh側に

- Shirt Collar
- Shirt Open
- Shirt Open L
- Shirt Open R

が正常に表示された。

Morphの動作も問題なし。

### 重要

Diffeomorphicで衣装Favoriteが読み込まれず、Import時に長いクラス名関連のエラーが出る場合、

`DAZ側のNode名が長すぎないか`

を確認する。

今回の問題はNode名短縮で解決した。

---

## 8. ハイヒールはニュートラル姿勢でマスターに保存

今回、Police / Office衣装用としてハイヒールを追加した。

ハイヒールをDAZで装着すると、足が自動的につま先立ちになる。

ただしBlender用マスターはニュートラル姿勢にしておきたいため、

`Edit > Figure > Zero > Zero Figure Pose`

でJiuyinを初期姿勢へ戻した。

ハイヒール自体は装着したまま非表示。

これにより、

- Jiuyin本体：ニュートラル姿勢
- High Heels：マスター内に存在
- 必要な時だけBlender側で表示

という形にできる。

---

## 9. 今回の完成状態

最終的に、Blender側のJiuyinマスターでは以下を確認できた。

- Hand / Finger Morph取り込み成功
- Police Shirt Morph取り込み成功
- Shirt Open / L / R 動作確認
- Shirt Collar動作確認
- High Heels取り込み成功
- Temptress一式取り込み成功
- Temptress系Merge Rigs成功
- High Heels Merge Rigs成功
- 衣装アーマチュアを整理
- Hair Curves化の手順を再確認
- 帽子を単独Asset化
- DAZ側Node名短縮でImportエラー解消

Outlinerもかなり整理された。

---

## 今後の作業

次は女性モデルを本番用に仕上げていく。

- Hair Meshの帽子干渉部分を削除
- Make Hair
- 帽子をAppend
- headボーンへ親子付け
- 足関節の回転制限解除
- CowGirlポーズアセット再適用
- 男性Genesis 9モデル取り込み
- 男女の簡易性器作成
- 男性器用リグ作成
- 監獄セットへ統合
- 1～181Fの騎乗位アニメーション再構築

DAZ→Blender間は、単純にモデルを持ってくるだけでも衣装・Morph・Hair・Rig周りでかなりハマりやすい。

ただ、今回の問題は一通り原因と解決方法を把握できた。

特に、

`Morphが来ない → DAZ FavoriteだけでなくNode名の長さも確認`

`衣装が個別Armature → Merge Rigs`

`髪が破綻 → Make Hair`

この3つは今後また使う可能性が高そう。
