---
layout: default
title: Cyberjutsu
---

# Welcome to Cyberjutsu

A cybersecurity blog dedicated to sharing practical knowledge across topics like network defense, penetration testing, and digital forensics.

---

> Thank you for visiting Cyberjutsu. Enjoy reading and learning!

---

## Categories

### Active Recon
{% assign active_recon_posts = site.posts | where: "categories", "Active-Recon" %}
{% for post in active_recon_posts %}
- [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}
