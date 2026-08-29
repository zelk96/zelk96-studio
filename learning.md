---
layout: default
title: Learning Log
permalink: /learning/
---

# Learning Log

Blenderとゲーム制作の学習・開発記録。新しい記事から順に掲載しています。

## Blender

{% assign blender_posts = site.pages | where: "learning_log", true | sort: "day" | reverse %}
{% for post in blender_posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) <span class="learning-date">— {{ post.date | date: "%Y.%m.%d" }}</span>
{% endfor %}

## Unity・ゲーム制作

- [敵を足場に変える「スタンプ」から始まった、2Dアクションゲーム『Palim』開発記]({{ '/posts/palim-stamp-system-development.html' | relative_url }})
- [UnityのTilemap上で敵が止まる原因はPhysics Material 2Dの摩擦だった]({{ '/posts/unity-tilemap-enemy-friction.html' | relative_url }})
- [Unityでジャンプ後の落下が遅い原因は、毎フレームY速度を0にしていたことだった]({{ '/posts/unity-rigidbody2d-preserve-y-velocity.html' | relative_url }})
- [Unityのブロック崩しでボールが水平・垂直に無限反射したときの調査記録]({{ '/posts/unity-ball-infinite-reflection.html' | relative_url }})
- [Unity初心者がAIと一緒に、約1週間でブロック崩しを完成させるまで]({{ '/posts/unity-beginner-breakout-with-ai.html' | relative_url }})

## Tools

- [ChatGPTのモデル・思考レベル選びを支援するChrome拡張を作った]({{ '/posts/modeguide-for-ai-chrome-extension.html' | relative_url }})
