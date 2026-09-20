---
layout: default
title: "瞬きアクションをテンプレ化する"
description: "決まったアクションを毎回キー打つのめんどいのでテンプレ化"
permalink: /posts/turn-the-blinking-action-into-a-template.html
date: "2026-09-19"
display_date: 2026.09.19
blender_note: true
---

# 問題

瞬きのような何度も繰り返す同じアクションを毎回キーフレーム打つのめんどいのでテンプレ化しよう

# 解決策

## Action Editorに登録する

1. 現在のモデル Armatureを選択して、Dope Sheet を Action Editor に切り替える
2. New を押して、新しいAction名をつける（Blink_Templateなど）
3. Action Editorで Template ができたら、Action名の横にある盾/Fake UserをONにしておく
   ※これをやらないと、そのActionをどこにも使っていない状態で保存→再読込したとき、消える可能性がある。

![ActionEditorに登録](/zelk96-studio/images/blender-note/20260919/02.png)

## Graph EditorでAuto Clampedを設定する

1. Graph Editorでこの3キーを選んで、V → Auto Clamped にしておく
   ※普通のBezierでも動くけど、瞬き値が0未満や1以上へ変にオーバーシュートするのを防ぎやすい。
   ※Graph Editorの選択物のみ表示がONになっていると、キーが表示されないことがある。

![自動固定](/zelk96-studio/images/blender-note/20260919/03.png)
![選択物のみ表示](/zelk96-studio/images/blender-note/20260919/04.png)

## NLA Editorで Template を NLA Strip化する

1. ノンリニアアニメーション（NLA Editor）を開く
2. Template がアクティブActionになってる状態で、Action Editor側で アクションをストリップ化（Push Down）を押す

![アクションをストリップ化](/zelk96-studio/images/blender-note/20260919/05.png)

完成！

## 作成したテンプレを使用する

まず モデルの Armatureを選択したまま、下のエリアを NLA Editor に切り替える。
そのあと本編ファイルのActionはPush Downしない。ここ超重要。

1. File → Append → makeActions.blend → Action から作成したActionファイルを読み込む
   今回は以下6つ

- Idle_Breath
- Idle_WeightShift
- Idle_HeadMicro
- Blink_Normal
- Blink_Slow
- Blink_Double

2. モデルの Armature を選択
3. Python Consoleを開いて、以下コードを実行して空のトラックを作成する

```python
obj = bpy.context.active_object; track = obj.animation_data.nla_tracks.new(); track.name = "TR_Idle_Breath"
```

まとめて追加する場合は

```python
obj = bpy.context.active_object
for name in ["TR_Idle_WeightShift", "TR_Idle_HeadMicro", "TR_Blink"]:
    if not obj.animation_data.nla_tracks.get(name):
        track = obj.animation_data.nla_tracks.new()
        track.name = name
```

4. できたTrackを選択
5. 追加 → アクション
6. まず Idle_Breath を選ぶ
7. そのStripの設定を

- Blend = Add
- Influence = 0.7〜1.0
- Repeat = シーン長に合う数

8. 同じやり方で別Trackに

- Idle_WeightShift
- Idle_HeadMicro

を追加

## 瞬きテンプレ（24fps）

### 通常

- 開眼：0F
- 全閉：3F
- 開眼：7F
- 1回：約0.3秒

### 間隔

- 通常：3〜6秒（72〜144F）
- 落ち着いたキャラ：4〜7秒（96〜168F）
- 緊張・活発：2〜4秒（48〜96F）
- 一定間隔にしない

### コツ

- 閉じる方を速く、開く方を少し遅くする
- 視線を変える直前・直後に瞬きを入れると自然
- 強い動作の最中より、動作の切れ目に入れやすい
- 長いシーンではたまに二度瞬きを混ぜる
