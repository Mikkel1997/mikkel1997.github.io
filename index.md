---
layout: default
---
<h1>Velkommen til min Portef�lje</h1>
<p>Her kan du f�lge med i min l�ring og udvikling gennem semestret.</p>

<h2>Mine Indl�g</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>