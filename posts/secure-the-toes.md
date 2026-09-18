---
layout: default
title: "IK TargetのEmptyをつま先側に移動させたい"
description: "足裏つま先側を地面に固定したかったが、IK Targetの置き方がわからず困ったのでメモ"
permalink: /posts/secure-the-toes.html
date: "2026-09-18"
display_date: 2026.09.18
blender_note: true
---

# モデルの足を地面に固定したい

足裏つま先側を地面に固定したかったが、IK Targetの置き方がわからず困った

## 解決方法

1. IK_Toe_R_Target を選択
2. オブジェクトコンストレイントで 「位置コピー（Copy Location）」 を追加
3. Target：Juyin Zhu
4. Bone：r_foot
5. Boneを指定すると出る Head/Tail を 1.000 にする

- 0.000 = Head（足首側）
- 1.000 = Tail（つま先側）

6. Emptyが r_foot の先端へ移動したら、コンストレイントを適用
7. これでEmptyを再び独立させる

![TargetをTail側へ](/zelk96-studio/images/blender-note/20260918/01.png)
