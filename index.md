---
layout: home
title: Home
---
# Dr Deepak Kumar Panda

**Cybersecurity · Trustworthy AI · Autonomous Systems**

I research resilient and trustworthy autonomous systems, with interests in AI/ML assurance, cybersecurity, adversarial machine learning, reinforcement learning, and cyber-physical systems.

My research connects learning-enabled decision-making with safety, security, and reliability under uncertain and adversarial conditions. Earlier work explored smart grids, networked control, digital twins, and energy systems.

**Affiliation:** Cranfield University, United Kingdom (verify current title before publishing).

[Research](research.html) · [Publications](publications.html) · [Technical blog](blog.html) · [Google Scholar](https://scholar.google.com/citations?user=dfY4ZvkAAAAJ&hl=en) · [ORCID](https://orcid.org/0009-0003-9925-4448)

## Recent writing

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: '%d %B %Y' }}
{% endfor %}
