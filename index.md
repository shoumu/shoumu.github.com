---
layout: default
title: 半天成
tagline:
---
{% include JB/setup %}
 
<!--   
<h2>最新文章</h2>


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
-->

<div class="posts">
  {% for post in site.posts %}
  {% assign content = post.content %}
  <section id="{{ post.url | replace:'/',''}}">
    <article>
      <header>
      <h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>
      <div class="date">{{ site.author.name }} 发表于 <span>{{ post.date | date:"%Y-%m-%d" }}</span></div>
    </header>

    <br />

    <!--
    <div class="content">{{ content }}</div>

    <br />
    <br />
    <br />
    <br />
    -->

    </article>
  </section>
  {% endfor %}
  </div>
