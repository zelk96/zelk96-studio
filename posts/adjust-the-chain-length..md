---
layout: default
title: "チェーンの長さを切り替える"
description: "現在Fまでチェーン3で使っていたIKを、次Fからチェーン2で使いたい"
permalink: /posts/adjust-the-chain-length..html
date: "2026-09-19"
display_date: 2026.09.19
blender_note: true
---

# 問題

これまでチェーン3として使っていたIKを次Fからチェーン2に変えると、それまでの全Fがチェーン2になってしまうので壊れる
そんなときの解決法

# 解決策

IKコンストレイントを2個用意してInfluenceで切り替えるのが安全。

具体的には

- IK①：チェーン長 3
- IK②：チェーン長 2

にする。

そして

10F

- IK① Influence = 1
- IK② Influence = 0

11F

- IK① Influence = 0
- IK② Influence = 1

のようにする。

これなら

- 〜10F：チェーン3
- 11F〜：チェーン2

で切り替えられる。

重要なのは、Influenceはそれぞれ個別にキーを打つこと。全ボーン I では保存されない。

あと今回は完全なスイッチ用途だから、Influenceの補間は Constant（一定） にしていい。
そうすれば10→11の間で「チェーン3と2が半々で効く」みたいな変な状態にもならない。

# 具体例

1. r_foot の今のIKコンストレイントを複製
2. 片方をチェーン3
3. もう片方をチェーン2
4. 348Fで 3=1 / 2=0
5. 349Fで 3=0 / 2=1

![IK設定](/zelk96-studio/images/blender-note/20260919/01.png)
