---
layout: page
permalink: /blog
title: Blog
---

<!-- adapted from https://github.com/MvvmCross/MvvmCross/blob/master/docs/_layouts/blog.html -->
<section class="blog">
    {% for post in site.posts %}
        <time class="time">
            {% assign date_format = site.date_format | default: "%b %-d, %Y" %} {{ post.date | date: date_format }}
        </time>
        <h2>
            <a class="link" href="{{ post.url | relative_url }}" role="link">{{ post.title | escape }}</a>
        </h2>
        <p class="meta">
            {{ post.content | strip_html | escape | truncatewords: 80 }}
        </p>
    {% endfor %}
</section>