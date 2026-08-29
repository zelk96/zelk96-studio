---
layout: default
title: Home
---

<section class="hero">
  <p class="eyebrow">3DCG / GAME DEVELOPMENT</p>
  <h1>Learning by making.</h1>
  <p>Blenderとゲーム制作の学習記録。作ったもの、詰まったところ、覚えたことを残していく。</p>
  <p><a class="button" href="{{ '/learning/' | relative_url }}">Learning Logを見る</a></p>
</section>

## Latest

{% assign blender_posts = site.pages | where: "learning_log", true | sort: "day" | reverse %}
{% for post in blender_posts limit: 1 %}
### [{{ post.title }}]({{ post.url | relative_url }})

<p class="learning-date">投稿日：{{ post.date | date: "%Y.%m.%d" }}</p>

{{ post.description }}
{% endfor %}
