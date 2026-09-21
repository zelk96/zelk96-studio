---
layout: default
title: "Convert To MHXで"
description: "Simple IKだけだとやれることが限られてくるのでMHXを導入した"
permalink: /posts/convert-to-mhx.html
date: "2026-09-21"
display_date: 2026.09.21
blender_note: true
---

# 問題

今後

- 立ち
- 座り
- 膝立ち
- 寝姿勢
- 接触動作
- 全身の芝居

などを大量に使うとなるとSimple IKだけでは足りなくなる。

# やったこと

## 導入

1. DAZ Setup → Rigging → Convert To MHX
2. 今回は以下を設定して実行

- Pole Targets → ON
- Stretchy Limbs → OFF
- Split Shin Vertex Group → ON
- Tweak Bones → OFF
- Keep DAZ Rig → OFF
- Finger IK → OFF
- Spine IK → OFF
- Tongue Control → なし
- Shaft Control → なし
- Ankle IK → OFF
- Keep Genesis 9 Twist Bones → OFF
- Genesis 3 Tarsals → どっちでも実質関係ないけど、Genesis 9だからOFFでOK
- Display Transform Bones → ON
- Missing Bone Errors → ON

Pole Targets をONにすると、その下にElbow Parent / Knee Parentが出るので

- Elbow Parent = Master
- Knee Parent = Master

を設定

## 今回使う主要コントローラ

ざっくりこのセットを覚えれば十分

| 用途       | MHX Bone               |
| ---------- | ---------------------- |
| キャラ全体 | master                 |
| 骨盤・重心 | pelvis                 |
| 腹～腰     | spine, spine-1         |
| 胸郭       | chest, chest-1         |
| 首         | neck, neck-1           |
| 頭         | head                   |
| 左足IK     | foot.ik.L              |
| 右足IK     | foot.ik.R              |
| 左膝Pole   | knee.pt.ik.L           |
| 右膝Pole   | knee.pt.ik.R           |
| 左腕IK     | hand.ik.L              |
| 右腕IK     | hand.ik.R              |
| 胸         | pectoral.L, pectoral.R |

### 3Dビューに見えてるコントローラ

- 足元の巨大な黄色い形 → master
- 足を囲んでる赤青の箱 → foot.ik.L/R
- 膝前方の赤青の丸 → knee.pt.ik.L/R
- 腰周りの黄色い箱 → pelvis/body系
- 胸や首周辺の黄色いライン → spine/chest/head系
- 手の赤青コントローラ → arm IK

つまり、普段はOutlinerの巨大な階層を開いて探す必要すらない。
画面に出てるCustom Shapeを直接触る運用になる。

## 操作ルールを固定

- master：配置だけ。アニメ中ほぼ触らない
- hip：主運動。前後・上下・骨盤角度
- back：胴体全体のしなり
- pelvis：最後の局所補正だけ
- foot.ik.L/R：接地点。基本固定
- knee.pt.ik.L/R：膝向き。基本固定
- head：頭の反応
- pectoral.L/R：最後に二次揺れ
