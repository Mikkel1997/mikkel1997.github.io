---
layout: default
title: Forside
---

# Velkommen til min Portefølje
Her kan du følge med i min læring indenfor Datascience, ML og AI gennem semestret.

## Mine Indlæg


# Kategorier

{% for category in site.categories %}
## {{ category[0] }}
<ul>
  {% for post in category[1] reversed %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span>— {{ post.date | date: "%d-%m-%Y" }}</span>
    </li>
  {% endfor %}
</ul>
{% endfor %}