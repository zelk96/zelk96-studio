---
layout: default
title: "腰に手を追従させる方法"
description: "腰の動きに合わせて手の動きを追従させたいときに"
permalink: /posts/make-the-hand-follow-the-waist.html
date: "2026-09-18"
display_date: 2026.09.18
blender_note: true
---

# 腰の表面に専用Emptyを置いて、それをIKターゲットにするのが一番きれい

手順はこんな感じ。

1. 今の手の位置、つまり「腰に自然に置けてる位置」にEmptyを作る
2. そのEmptyを pelvis に追従させる
3. Child Of でもいいし
4. Copy Transforms / Copy Locationでもいい
5. IKのTargetを pelvis からそのEmptyへ変更
6. IK影響を0→1に切り替えても手がズレない位置までEmptyを微調整

![Empty設定](/zelk96-studio/images/blender-note/20260918/05.png)
![IK設定](/zelk96-studio/images/blender-note/20260918/06.png)

要するに

- pelvis = 腰全体の基準
- Hand_R_Waist_Target = 腰の表面にある手の接触点
