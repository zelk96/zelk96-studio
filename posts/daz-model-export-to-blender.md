---
layout: default
title: "DAZモデルをDiffeomorphicでBlenderへエクスポートする方法"
description: "DAZ StudioのGenesis 9モデルをDUFとDBZで保存し、DiffeomorphicのEasy Import DAZからBlenderへ読み込む手順と注意点の備忘録。"
permalink: /posts/daz-model-export-to-blender.html
date: "2026-09-13"
display_date: 2026.09.13
blender_note: true
note_order: 1
---

# DAZモデルをDiffeomorphicでBlenderへエクスポートする方法

DAZ Studioで作成したGenesis 9モデルを、ボーン、衣装、マテリアル、モーフを維持したままBlenderへ渡すための備忘録。

通常のOBJやFBXではなく、Diffeomorphic DAZ ImporterのDAZ側スクリプトとBlender側アドオンを使用する。

## 使用環境

- Blender 5.2.0 LTS
- Diffeomorphic 5.2.0.3018
- Genesis 9

DAZ側のエクスポート用スクリプトとBlender側アドオンは、できるだけ同じ配布版を使用する。

## 1．DAZ Studioでモデルを完成させる

Genesis 9へ必要なキャラクター、衣装、髪、マテリアルを適用する。

書き出す前に次の項目を確認する。

- 必要なフィギュアがSceneに存在する
- 衣装と髪が正しいフィギュアへFitしている
- 不要なライト、カメラ、背景を整理している
- Blenderでも必要になる身体部分が残っている
- 保存先のフォルダとファイル名を決めている

後から衣装を外して使う可能性がある場合は、衣装ありの完成モデルとは別に、ベースモデルだけのDAZシーンも保存しておくと安全である。

![DAZStudioで書き出すモデルを選択した状態]({{ '/images/guides/daz-export/01.png' | relative_url }})

## 2．シーンをDUFで保存する

DAZ Studioの`File → Save As → Scene`から、シーンを`.duf`形式で保存する。

例：

```text
D:\3DCG\BlenderAssets\Characters\Genesis9Male\BaseMasculine\
└─ G9M_BaseMasculine.duf
```

DUFはDAZシーン本体であり、後でBlenderのファイル選択画面から指定するファイルになる。

## 3．DiffeomorphicのExport Blenderを実行する

DAZ StudioのContent Libraryで次の場所を開く。

```text
My DAZ 3D Library
└─ Scripts
   └─ Diffeomorphic
      └─ Export Blender
```

`Export Blender`を実行し、先ほど保存したDUFと同じフォルダへ補助データを書き出す。

正常に完了すると、主に次のファイルが並ぶ。

```text
G9M_BaseMasculine.duf
G9M_BaseMasculine.dbz
G9M_BaseMasculine.duf.png
G9M_BaseMasculine.tip.png
```

重要なのは`.duf`と`.dbz`のベース名と保存場所を一致させること。Blender側ではDUFを選択するが、Diffeomorphicは同じ場所にあるDBZも参照する。

![DUFとDBZを書き出したフォルダ]({{ '/images/guides/daz-export/02.png' | relative_url }})

## 4．BlenderでEasy Import DAZを実行する

Blenderを開き、サイドバーの`DAZ Setup`から`Easy Import DAZ`を実行する。

ファイル選択画面で、DAZ Studioから保存した`.duf`を選択する。`.dbz`を直接選ぶわけではない。

インポートが始まったら、処理が完了するまで待つ。モデル、衣装、髪、マテリアル、モーフが多いほど時間がかかる。

## 5．読み込み結果を確認する

インポート後は、次の項目を確認する。

- アーマチュアと身体メッシュが存在する
- 衣装、目、口、眉、まつ毛、髪が揃っている
- マテリアルプレビューまたはレンダーで色が出る
- ポーズモードで手足を動かせる
- 衣装が身体へ追従する
- DAZ SetupとDAZ Runtimeが対象アーマチュアを認識している
- 必要な表情モーフが残っている

表示できただけで完了とは判断せず、実際にボーンを動かし、テストレンダーまで行う。

![Blenderへ読み込んだGenesis9モデル]({{ '/images/guides/daz-export/03.png' | relative_url }})

## OBJ・FBXとの違い

DAZ Studioの通常の`File → Export`からOBJを書き出す方法は、静止したメッシュを渡す用途には使える。

しかしOBJでは、アーマチュア、ウェイト、モーフなどのキャラクター制御情報が維持されない。Blender上でポーズやアニメーションを作るモデルには向かない。

FBXはボーン付きで書き出せるが、DAZ固有のマテリアル、モーフ、ストランドベースヘアーなどが正しく再現されない場合がある。今回の用途ではDiffeomorphic経由の方が扱いやすかった。

## よくある問題

### DUFはあるがDBZがない

DAZシーンを保存しただけではDBZは作成されない。シーン保存後に、Diffeomorphicの`Export Blender`も実行する。

### BlenderでDUFを選択しても読み込めない

次の点を確認する。

- DUFとDBZが同じフォルダにある
- DUFとDBZのベース名が一致している
- DAZ側スクリプトとBlender側アドオンのバージョンが大きく違わない
- BlenderのDiffeomorphic設定でDAZ Libraryの場所が認識されている

### マテリアルやモーフの処理中にエラーが出る

一部だけ読み込まれている場合もあるため、アーマチュア、メッシュ、マテリアル、モーフを個別に確認する。

中途半端に読み込まれたシーンへ再実行するより、新しいBlendファイルで最初から読み直した方が問題を切り分けやすい。

### 靴を非表示にすると脚まで消える

靴の下の身体形状が、DAZ Studioからの書き出し時点ですでに含まれていない場合がある。

Blender側のモディファイアーだけが原因とは限らない。素足版が必要なら、DAZ Studioで靴を外したベースモデルを別シーンとして保存し、あらためてDUFとDBZを書き出す。

## 最小チェックリスト

1. DAZ Studioでキャラクターを完成させる
2. シーンを`.duf`として保存する
3. Diffeomorphicの`Export Blender`を実行する
4. DUFとDBZを同じ名前・同じフォルダへ置く
5. Blenderの`Easy Import DAZ`でDUFを選ぶ
6. ボーン、マテリアル、衣装、髪、モーフを確認する
7. 手足を動かし、テストレンダーを行う

ストランドベースヘアーは、Diffeomorphicで読み込んだだけでは最終レンダーに表示されない場合がある。その場合は`Make Hair`を使用し、レンダリング可能なヘアーカーブへ変換する。
