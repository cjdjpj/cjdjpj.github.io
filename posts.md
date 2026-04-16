---
layout: default
title: Posts
permalink: /posts/
---

<div id="home">
    <h1>Posts</h1>
    <p style="color: gray; font-style: italic; font-size: 12pt;">Sometimes I write things to help myself learn.</p>
    <ul class="posts">
        {% for post in site.posts %}
        <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endfor %}
    </ul>

    <!-- <h1>Publications</h1> -->
    <!-- <ul class="posts"> -->
    <!--   <li></li> -->
    <!-- </ul> -->
</div>

