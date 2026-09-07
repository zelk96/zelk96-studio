---
layout: default
title: "Blender学習29日目｜DAZ Studioを導入し、Genesis 9をIrayで初レンダー"
permalink: /posts/blender-day29.html
date: '2026-09-07'
display_date: 2026.09.07
learning_log: true
day: 29
---

# Blender学習29日目｜DAZ Studioを導入し、Genesis 9をIrayで初レンダー

昨日は、すべてを0から作ることよりも、まず売れる作品を完成させることを優先すると決めた。

今日はその方針転換の第一歩として、Daz Studio 6を導入した。Genesis 9のキャラクターへ髪、衣装、ポーズ、表情を適用し、カメラとHDRI背景、補助ライトを設定して、NVIDIA Irayによる静止画レンダーまで一通り試した。

途中では、必要なGenesis 9ファイルが一部しか入っていない、出力サイズを変更できない、ライトを追加したら逆に暗くなる、レンダーが真っ黒になるなど、初見では理由の分からない問題も発生した。

それでも最終的には、既存アセットから一枚の画像を作る基本工程を最後まで完了できた。

## Daz Install ManagerからDaz Studio 6を導入する

昨日ダウンロードしたDaz Install Managerを起動し、最初に表示された推奨コンポーネントをインストールした。

ところが、インストール後にWindowsで検索してもDaz Studio本体が見つからなかった。最初に入ったのは主にコンテンツであり、アプリ本体の`Daz Studio 6 (Win 64-bit)`は別にインストールする必要があった。

![Daz Install ManagerでDaz Studio 6をインストール](/zelk96-studio/images/day29/01.png)

アプリ本体を追加するとDaz Studio 6を起動できた。アカウント接続とメタデータの読み込みを行い、Smart Contentから利用可能な素材を確認した。

## Genesis 9 Starter Essentialsをすべて入れる

起動直後はGenesis 9のベースフィギュアが見つからず、人物を読み込めなかった。

Install Managerを確認すると、`Genesis 9 Starter Essentials`は三分割されており、インストール済みだったのは`3 of 3`だけだった。`Ready to Download`に残っていた`1 of 3`と`2 of 3`も追加でインストールした。

![Genesis 9 Starter Essentialsの不足パッケージを確認](/zelk96-studio/images/day29/02.png)

三つすべてがInstalledへ移ると、Content Libraryの`My DAZ 3D Library → People → Genesis 9`にGenesis 9のベースフィギュアが現れた。

![Genesis 9のベースフィギュアを読み込んだ状態](/zelk96-studio/images/day29/03.png)

Dazでは、同じ製品名のファイルが複数パッケージに分かれている場合がある。サムネイルが出ないときは、製品名だけでなく`1 of 3`のような分割番号まで確認した方がよいと分かった。

## Kat for Genesis 9をベースにする

無料で使えるGenesis 9キャラクターには、Amala、Fabrice、Kat、Laura、Matt、Tyが入っていた。

今回は将来的なゲーム寄りセミリアルの作品を想定し、その中では比較的方向性を合わせやすそうだった`Kat for Genesis 9`を選んだ。最初に読み込んだ汎用Genesis 9を削除してからKatを読み込んだ。

![無料キャラクターからKat for Genesis 9を選択](/zelk96-studio/images/day29/04.png)

無料素材だけでは狙ったかわいさには届かないが、最初から完成形を求める必要はない。今後は有料の顔、肌、髪、衣装などを組み合わせ、作品の方向性に合わせて調整していく。

## 髪と作業着を追加する

髪には`Eirgrid Hair G9 with dForce`を使用した。フィギュアを選択してから髪を読み込むと、Genesis 9へ追従する状態で装着された。

![Eirgrid Hairを装着したKat](/zelk96-studio/images/day29/05.png)

衣装には`Worker Uniform Outfit`を使用した。ジャンプスーツとインナーを読み込み、短時間で服を着たキャラクターを用意できた。

![Worker Uniform Outfitを装着](/zelk96-studio/images/day29/06.png)

3Dビューの空白をクリックして選択が外れたときは、右側の`Scene`タブからフィギュアを選び直せた。このSceneペインは、Blenderでいうアウトライナーに近い。

## ポーズと表情を試す

次に、プリセットからポーズを適用した。

`Space Valkyrie Poses`の`StandA`を選ぶと、両腕を大きく上げた非常に勢いのある姿勢になった。見た瞬間、レイザーラモンHGの「フォー！」にしか見えなかった。

![両腕を上げたSpace Valkyrieのポーズ](/zelk96-studio/images/day29/07.png)

さらにScream表情を重ねると、商品画像には到底使えない強烈な状態になった。表情は`CDI SVE !Reset`でリセットし、一般的な立ちポーズと`Smile MouthClosed`へ変更した。

![立ちポーズと口を閉じた笑顔へ変更](/zelk96-studio/images/day29/08.png)

完成度はともかく、ポーズと表情を別々に適用し、失敗した部分だけリセットできることは確認できた。

## 現在の視点からカメラを作る

構図を固定するため、`Create → New Camera`から`Camera_Main`を作成した。

作成ダイアログの`More...`を開き、`Apply Active Viewport Transforms : <Perspective View>`を選択した。これにより、現在見ているPerspective Viewの位置と向きをそのままカメラへコピーできる。

![現在のPerspective ViewからCamera_Mainを作成](/zelk96-studio/images/day29/09.png)

作成後は、ビューポート右上の表示を`Perspective View`から`Camera_Main`へ切り替えた。これでレンダーされる範囲を見ながら構図を調整できる。

## HDRI Starter Packで背景と環境光を入れる

Light Presetsには使用できそうな素材が見つからなかったため、Productsの`H → HDRI Starter Pack`を開いた。今回は近未来的な`DHDRI EXO Hangar`を適用した。

![DHDRI EXO Hangarを適用したビューポート](/zelk96-studio/images/day29/10.png)

背景がぼやけて見えたが、これは実際の3DセットではなくHDRI画像を環境として使っているためだった。HDRIは背景と周囲からの照明を手軽に用意できる一方、床や壁の実物メッシュが存在するわけではない。そのため、足元の接地感は別途工夫する必要がある。

## レンダーサイズを800×604へ変更する

レンダーエンジンは`NVIDIA Iray (MDL)`を使用した。

最初はRender SettingsのDimensionsで幅へ`800`を入力しても、すぐ`1623`へ戻ってしまった。原因は`Dimension Preset (Global)`が`Active Viewport`になっていたことだった。

プリセットを`Custom`へ変更してから幅を800にすると、比率を維持したまま高さが604になった。

![Dimension PresetをCustomにして800×604へ設定](/zelk96-studio/images/day29/11.png)

## Irayで最初の一枚をレンダーする

設定後、Irayで最初のテストレンダーを実行した。

![最初のIrayレンダー](/zelk96-studio/images/day29/12.png)

画像は`Day29_DAZ_Test01.png`として保存し、シーン本体も`Day29_DAZ_FirstScene.duf`として保存した。レンダー画像はRender Libraryから`Browse to File Location`を選ぶことで、OneDrive内の画像フォルダにある保存場所を確認できた。

この一枚で、キャラクター、衣装、ポーズ、カメラ、HDRI、レンダー、保存という最低限の流れは成立した。ただし、背景の強い光に対して人物の顔と服が暗く、主役としては見づらかった。

## ライトを追加したら逆に暗くなる

人物を明るくするため、`Key_Light`というスポットライトを作成した。カメラとほぼ同じ方向から照らすため、New Spotlightの`More...`から`Apply Active Viewport Transforms : <Camera_Main>`を選んだ。

ところが、光量を15,000ルーメンにしてレンダーしたTest02は、Test01より暗くなった。

![ライト追加後に暗くなったTest02](/zelk96-studio/images/day29/13.png)

これはシーンライトを追加したことで、Dazの自動的なカメラ用ヘッドランプが使われなくなり、代わりに追加したライトだけでは光量が足りなかったためだった。

そこで光量を150,000ルーメンに上げたTest03、さらに1,000,000ルーメンにしたTest04を作成した。人物は十分明るくなったが、Pointライトのままでは光が硬く、顔や衣装が平面的に見えた。

![光量を上げて人物を明るくしたTest04](/zelk96-studio/images/day29/14.png)

## Rectangleライトで柔らかい光を作る

光を柔らかくするため、Key_Lightの`Light Geometry`を`Point`から`Rectangle`へ変更し、HeightとWidthを100にした。

その直後のレンダーは全面が真っ黒になった。Rectangleの発光面がカメラと同じ位置にあり、`Render Emitter`がOnだったため、発光板そのものがカメラを塞いでいた。

![Rectangleライトの発光面で真っ黒になったレンダー](/zelk96-studio/images/day29/15.png)

`Render Emitter`をOffにするとライト本体は画像へ写らなくなり、照明効果だけを残せた。

![Rectangleライトで柔らかく照らした最終レンダー](/zelk96-studio/images/day29/16.png)

最終的なTest05では、Test04より顔と衣装の明暗が自然になった。まだ背景の白飛び、足元の接地感、キャラクター自体の品質など改善点は多いが、ライトの形状によって影とハイライトの硬さが変わることを実際に比較できた。

## 今日の振り返り

今日はDaz Studioを導入し、Genesis 9を使った初めてのシーンを完成させた。

学んだ基本工程は次の通りである。

- Genesis 9とキャラクターを読み込む
- 髪と衣装をフィギュアへ装着する
- ポーズと表情のプリセットを適用する
- Sceneペインから対象を選択する
- 現在の視点からカメラを作る
- HDRIで背景と環境光を用意する
- Irayでレンダーして画像を保存する
- 補助ライトの光量と形状を調整する

無料素材だけで理想のかわいいキャラクターを作るのは難しい。しかし、今日の目的は完成商品を作ることではなく、既存アセットを組み合わせて一枚の画像へ仕上げる制作経路を確認することだった。

その意味では、インストールから最終レンダーまで一周できたため、Day29の目標は達成できた。

特に、ライトを追加すると自動照明の状態が変わること、PointとRectangleでは光の質が異なること、発光面をカメラ位置へ置く場合はRender Emitterを切る必要があることは、実際に失敗したからこそ理解できた。

## 次回

次回は、商品化を意識したキャラクター作りへ進む。

まずは販売候補となるGenesis 9対応のキャラクター、肌、髪、衣装などを調査し、ゲーム寄りセミリアルでかわいく見せられる組み合わせを考えたい。無料素材で操作を覚える段階から、売れる画面を作るための素材選定と演出へ移っていく。
