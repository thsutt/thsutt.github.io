---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Tom's Blog
---
# Welcome to My Blog
This is the custom homepage of my Jekyll site.

## Recent Posts
<ul>
  {% for post in site.posts limit:3 %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>

## About Me
I am a web developer and this is my blog where I share my thoughts and projects.