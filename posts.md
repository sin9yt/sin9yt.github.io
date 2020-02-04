---
layout: page
title: Posts
---

* * *

<div style="font-size: 2rem;font-family: 'EB Garamond', serif;color: #caad00;">CTF Writeups </div> 
<p>
  <ul style="font-size: 1rem">
  {% for post in paginator.posts %}
    {% if post.type == "writeup" %}
      <li><a href="{{ site.base }}{{ post.url | remove_first: '/'}}">{{ post.title }} - {{ post.category }} {{post.points}}</a></li>
    {% endif %}
  {% endfor %}
</ul>
</p>

<div style="font-size: 2rem;font-family: 'EB Garamond', serif;color: #caad00;">Articles</div>
  
<p>
  <ul style="font-size: 1rem">
  {% for post in paginator.posts %}
    {% if post.type == "article" %}
      <li><a href="{{ site.base }}{{ post.url | remove_first: '/'}}"></a></li>
    {% endif %}
  { %endfor %}
</ul>
</p>
