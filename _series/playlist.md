---
layout: default
title: "Series: playlist"
series: playlist
---
{% comment %}
{% for list in site.series %}
  <a href="/blog/{{ list.url }}">{{ list.title }}&nbsp;</a>
{% endfor %}
{% endcomment %}


<div class="post content-div">
  {% assign s = "" %}
  {% for post in site.posts %}
    {% if post.series %}
      {% assign a = post.series | split:" " %}
      {% assign s1 = a | last | append:"|" | append: post.title | append: "|" | append: post.url %}
      {% assign s = s | append: s1 | append: "#"  %}
    {% endif %}
  {% endfor %}
{% assign s = s | split: "#" | sort %}
<h1>{{ page.title }}</h1>
<ol>
{% for item in s %}
  {% assign arr = item | split: "|" %}
  <li><a href="{{ site.baseurl }}{{ arr[2] }}">{{ arr[1] }}</a></li>
{% endfor %}
</ol>

</div>
