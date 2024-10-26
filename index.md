---
layout: default
title: Cyberjutsu
---

# Welcome to Cyberjutsu

A collection of tech write-ups, guides, and reflections shared with the community. This repository hosts the markdown files that are automatically turned into blog posts via GitHub Pages.

## About

This blog is dedicated to sharing knowledge on various topics such as cybersecurity, software development, and system administration. The content ranges from beginner-friendly tutorials to deep dives into complex subjects.

## Usage

The blog is publicly accessible at [gnar1zard.github.io/cyberjutsu](https://gnar1zard.github.io/cyberjutsu). Feel free to explore the posts and share your thoughts.

## Feedback

Your feedback is valuable! If you have any comments or would like to discuss a post's content, please open an issue in the repository.

---

> Thank you for visiting Cyberjutsu. Enjoy reading and learning!

---

## Categories

### Active Recon
{% assign active_recon_posts = site.posts | where: "categories", "Active-Recon" %}
{% for post in active_recon_posts %}
- [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}
