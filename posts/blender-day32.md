---
layout: default
title: "Blender学習32日目：独房の完成とオリジナル制帽制作"
permalink: /posts/blender-day32.html
date: "2026-09-10"
display_date: 2026.09.10
learning_log: true
day: 32
---

# Blender学習32日目：独房の完成とオリジナル制帽制作

今日は制作していた監獄部屋をひとまず完成とした。

壁や床の経年劣化、照明の明るさなどを調整し、暗く退廃的な独房らしい雰囲気に仕上げた。

![監獄部屋完成](/zelk96-studio/images/day32/01.png)

続いて、DAZ StudioでGenesis 9モデルに衣装と髪を装着し、FBX形式でBlenderへ読み込んだ。

さらにPythonスクリプトで制帽のベースを生成。生成後はクラウンのシルエット、ツバ、装飾の位置などを手作業で調整し、衣装に合う紺色の制帽に仕上げた。

![帽子完成](/zelk96-studio/images/day32/02.png)

最後にモデルを監獄部屋へ配置して、全体のサイズ感を確認した。

![配置テスト](/zelk96-studio/images/day32/03.png)

髪のテクスチャと透過、白目になっている目のマテリアルなど、FBX読み込み時の問題が残っているため、次回はその修正から進める。
