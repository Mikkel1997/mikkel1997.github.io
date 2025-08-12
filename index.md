---
layout: default
---
<h1>Velkommen til min Portefølje</h1>
<p>Her kan du følge med i min læring og udvikling gennem semestret.</p>

<h2>Mine Indlæg</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>