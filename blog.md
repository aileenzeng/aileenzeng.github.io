---
layout: page
permalink: /blog
title: Blog
---

<!-- adapted from https://github.com/MvvmCross/MvvmCross/blob/master/docs/_layouts/blog.html -->
<section class="blog">
    {% for post in site.posts %}
        <h2 class="post-title p-name">
            <a class="link" href="{{ post.url | relative_url }}" role="link">{{ post.title | escape }}</a>
        </h2>
        <p class="post-meta">
            <time class="dt-published" itemprop="datePublished">
                {% assign date_format = site.date_format | default: "%b %-d, %Y" %} {{ post.date | date: date_format }}
            </time>
        </p>
        <p class="post-content">
            {{ post.content | strip_html | escape | truncatewords: 80 }}
        </p>
    {% endfor %}
</section>