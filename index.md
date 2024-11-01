---
layout: default
title: Cyberjutsu
---

# Welcome to Cyberjutsu

A cybersecurity blog dedicated to sharing practical knowledge across topics like network defense, penetration testing, and digital forensics.

---

### Disclaimer

**Legal and Ethical Considerations:** All content provided on this site is intended for educational and informational purposes only. Always obtain proper authorization before conducting any cybersecurity activities, including network testing, vulnerability assessment, and penetration testing, to avoid violating legal or ethical boundaries.

---

> Thank you for visiting Cyberjutsu. Enjoy reading and learning!

---

## Categories

### Active Recon

{% assign active_recon_posts = site.posts | where: "categories", "Active-Recon" %}
{% for post in active_recon_posts %}

- [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}

### Web-App

{% assign web_app_posts = site.posts | where: "categories", "Web-App" %}
{% for post in web_app_posts %}

- [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}

### Client-Side

{% assign client_side_posts = site.posts | where: "categories", "Client-Side" %}
{% for post in client_side_posts %}

- [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}
