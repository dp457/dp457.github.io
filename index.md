---
layout: page
title: ""
permalink: /
---

<div class="academic-hero">
  <div class="hero-content">

    <p class="hero-eyebrow">
      CYBERSECURITY · TRUSTWORTHY AI · AUTONOMOUS SYSTEMS
    </p>

    <h1>Dr Deepak Kumar Panda</h1>

    <h2>
      Advancing the Security and Assurance
      of Autonomous Systems
    </h2>

    <p>
      My research focuses on cybersecurity, artificial
      intelligence, reinforcement learning, and the
      safety and assurance of autonomous systems.
      I investigate how learning-enabled systems
      can operate reliably under uncertainty,
      adversarial conditions, and emerging
      cybersecurity threats.
    </p>

    <p>
      My broader research interests include
      adversarial machine learning, cyber-physical
      systems, resilient decision-making, and
      trustworthy autonomous intelligence.
    </p>

    <div class="hero-links">
      <a class="hero-button primary"
         href="{{ '/research.html' | relative_url }}">
        Explore Research
      </a>

      <a class="hero-button secondary"
         href="{{ '/publications.html' | relative_url }}">
        Publications
      </a>
    </div>

    <div class="academic-social">
      <a href="https://scholar.google.com/citations?user=dfY4ZvkAAAAJ">
        Google Scholar
      </a>
      <a href="https://orcid.org/0009-0003-9925-4448">
        ORCID
      </a>
      <a href="https://github.com/dp457">
        GitHub
      </a>
    </div>

  </div>

  <div class="hero-photo">
    <img
      src="{{ '/assets/images/88123992.png' | relative_url }}"
      alt="Portrait of Deepak Kumar Panda">
  </div>
</div>

<section class="homepage-section">
  <h2>Research Areas</h2>

  <div class="research-grid">

    <div class="research-card">
      <h3>Cybersecurity of Autonomous Systems</h3>
      <p>
        Cyber threats, adversarial attacks,
        system resilience, and security assurance
        for autonomous cyber-physical systems.
      </p>
    </div>

    <div class="research-card">
      <h3>Trustworthy and Resilient AI</h3>
      <p>
        Robust reinforcement learning, adversarial
        machine learning, distribution shifts,
        and reliable AI-enabled decision-making.
      </p>
    </div>

    <div class="research-card">
      <h3>AI and System Safety Assurance</h3>
      <p>
        Safety-critical autonomy, cybersecurity
        assurance, and evidence-based evaluation
        of learning-enabled systems.
      </p>
    </div>

  </div>
</section>

<section class="homepage-section">
  <h2>Selected Publications</h2>

  <div class="featured-publication">
    <h3>
      Curriculum-Adapted Robust Reinforcement Learning
      for UAV Deconfliction in Adversarial Environments
    </h3>
    <p>IEEE Transactions on Reliability, 2026</p>
  </div>

  <div class="featured-publication">
    <h3>
      Meta Policy Switching for Resilient UAV
      Navigation in Adversarial Airspace
    </h3>
    <p>Information Sciences, 2026</p>
  </div>

  <div class="featured-publication">
    <h3>
      Action Robust Reinforcement Learning for
      Air Mobility Deconfliction Against
      Conflict Induced Spoofing
    </h3>
    <p>
      IEEE Transactions on Intelligent
      Transportation Systems, 2024
    </p>
  </div>

  <p>
    <a href="{{ '/publications.html' | relative_url }}">
      View all publications →
    </a>
  </p>
</section>

<section class="homepage-section">
  <h2>Latest Technical Writing</h2>

  {% for post in site.posts limit:3 %}
  <div class="blog-preview">
    <p class="blog-date">
      {{ post.date | date: "%d %B %Y" }}
    </p>
    <h3>
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
    </h3>
    <p>{{ post.excerpt | strip_html | truncatewords: 35 }}</p>
  </div>
  {% endfor %}

  <p>
    <a href="{{ '/blog.html' | relative_url }}">
      Explore the technical blog →
    </a>
  </p>
</section>
