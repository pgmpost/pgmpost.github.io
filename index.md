---
layout: default
title: Programming Post
---

# The Programming Post

> A blog about programming !
> -- [RSS feed](https://pgmpost.github.io/feed.xml)

<hr>

![programming-pic](https://www.freecodecamp.org/news/content/images/2022/12/main-image.png)

<br>

You can find all posts ordered by date below :

### All Posts

<ul>
  {% for post in site.posts %}
    <li>
      {{ post.date | date_to_string }} <a href="{{ post.url }}">{{ post.title }}</a> 
    </li>
  {% endfor %}
</ul>
