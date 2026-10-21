---
layout: page
title: Technical Blog
permalink: /blog/
---

<section class="blog-landing">
  <p class="blog-eyebrow">IDEAS · METHODS · TUTORIALS</p>
  <h1>Technical Blog</h1>
  <p>Accessible explanations and practical tutorials on AI security, autonomous systems, reinforcement learning, estimation, and assurance.</p>
</section>

<div class="blog-controls">
  <label for="blog-search" class="blog-search-label">Search articles</label>
  <input id="blog-search" type="search" placeholder="Search by title, topic, or keyword…" autocomplete="off">
  <label for="blog-category" class="blog-filter-label">Topic</label>
  <select id="blog-category">
    <option value="all">All topics</option>
    {% assign all_categories = site.posts | map: "categories" | join: "," | split: "," | uniq | sort %}
    {% for category in all_categories %}
      {% assign cleaned = category | strip %}
      {% if cleaned != "" %}
      <option value="{{ cleaned | downcase | escape }}">{{ cleaned | escape }}</option>
      {% endif %}
    {% endfor %}
  </select>
  <p id="blog-count" class="blog-count" aria-live="polite"></p>
</div>

<div id="blog-list" class="blog-grid">
{% for post in site.posts %}
  {% assign post_excerpt = post.description | default: post.excerpt %}
  <article class="blog-card"
    data-search="{{ post.title | append: ' ' | append: post.description | append: ' ' | append: post.tags | append: ' ' | append: post.categories | downcase | escape }}"
    data-categories="{% for cat in post.categories %}|{{ cat | downcase | strip | escape }}{% endfor %}|">
    <div class="blog-card-meta">
      <time datetime="{{ post.date | date: '%Y-%m-%d' }}">{{ post.date | date: "%d %B %Y" }}</time>
      {% if post.categories.size > 0 %}
      <span class="blog-category-label">{{ post.categories | first | escape }}</span>
      {% endif %}
    </div>
    <h2><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h2>
    <p>{{ post_excerpt | strip_html | normalize_whitespace | truncatewords: 32 }}</p>
    {% if post.tags.size > 0 %}
    <div class="blog-tags">
      {% for tag in post.tags limit:3 %}<span>{{ tag | escape }}</span>{% endfor %}
    </div>
    {% endif %}
    <a class="blog-read" href="{{ post.url | relative_url }}" aria-label="Read {{ post.title | escape }}">Read article →</a>
  </article>
{% endfor %}
</div>

<p id="blog-empty" class="blog-empty" hidden>No articles match your search. Try another topic or keyword.</p>
{% if site.posts.size == 0 %}
<p class="blog-empty-static">Articles are coming soon. Check back for tutorials and technical explainers.</p>
{% endif %}

<script>
(function () {
  const search = document.getElementById('blog-search');
  const category = document.getElementById('blog-category');
  const cards = Array.from(document.querySelectorAll('.blog-card'));
  const count = document.getElementById('blog-count');
  const empty = document.getElementById('blog-empty');
  function update() {
    const q = search.value.trim().toLowerCase();
    const c = category.value;
    let shown = 0;
    cards.forEach(function (card) {
      const matches = card.dataset.search.includes(q) &&
        (c === 'all' || card.dataset.categories.includes('|' + c + '|'));
      card.hidden = !matches;
      if (matches) shown++;
    });
    count.textContent = shown + ' article' + (shown === 1 ? '' : 's');
    empty.hidden = shown !== 0 || cards.length === 0;
  }
  search.addEventListener('input', update);
  category.addEventListener('change', update);
  update();
}());
</script>
