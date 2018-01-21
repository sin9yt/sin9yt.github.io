---
layout: default
title: home
---

# $ cat about.txt
{:id="about"}

This is a theme intended to use to [lampiaosec](https://lampiaosec.github.io). So, it's our taste, but because we like free culture, it's free to.

The categories to post was to support us, but you can change as you need.

# $ cat contact.txt
{:id="contact"}

E-mail:
	
> sindhoorhegde378[at]gmail[dot]com

PGP:

> [E106 60C0 DC06 98D7 EBA3  0E26 CC20 47C7 1F4C E758](/assets/pub.key.asc)

Twitter:

> [@sin9yt](https://twitter.com/sin9yt)

GitHub:

> [sin9yt](https://github.com/sin9yt)

Bitcoin: 
	
> 1E4cbGnqx4RRR3Lb2rJV1mohoErnWyHe9g

# $ cat projects.txt
{:id="projects"}

<ul>
{% for project in site.categories.projects %}
<li><a href="{{ project.link }}">{{ project.title }}</a> - {{ project.description }}</li>
{% endfor %}
</ul>

# $ cat tools.txt
{:id="tools"}

<ul>
{% for tool in site.categories.tools %}
<li><a href="{{ tool.link }}">{{ tool.title }}</a> - {{ tool.description }}</li>
{% endfor %}
</ul>

# $ cat talks.txt
{:id="talks"}

<ul>
{% for talk in site.categories.talks %}
<li><a href="{{ talk.link }}" title="{{ talk.description }}">{{ talk.title }}</a> at {{ talk.where }}</li>
{% endfor %}
</ul>

# $ cat posts.txt
{:id="posts"}

<ul>
{% for post in site.categories.posts %}

{% if post.en %}
<li>{{ post.title }} :: <a href="{{ post.url }}" title="{{ post.description }}">en</a> :: <a href="{{ post.pt }}" title="{{ post.description_pt }}">pt_br</a></li>
{% endif %}

{% endfor %}
</ul>

# $ cat articles.txt
{:id="articles"}

<ul>
{% for post in site.categories.articles %}

{% if post.en %}
<li>{{ post.title }} :: <a href="{{ post.url }}" title="{{ post.description }}">en</a> :: <a href="{{ post.pt }}" title="{{ post.description_pt }}">pt_br</a></li>
{% endif %}

{% endfor %}
</ul>
