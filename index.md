---
layout: default
title: Forside
---

# Velkommen til min Portefølje
Her kan du følge med i min læring og udvikling gennem semestret.

## Mine Indlæg
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span>— {{ post.date | date: "%d-%m-%Y" }}</span>
    </li>
  {% endfor %}
</ul>