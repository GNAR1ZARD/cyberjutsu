---
layout: default
title: "Active Recon"
---

<h1>Active Recon</h1>
{% for post in site.categories.Active-Recon %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}
