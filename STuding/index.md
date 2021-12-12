---
layout: main
title: STuding
main: true
---

<div class="loading-animation">

{% include hashtag.html %}

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'date' | reverse | where: 'type', 'STuding' %}
{% for page in sorted %}
{% include post-list.html %}
{% endfor %}
</ul>
</div>

