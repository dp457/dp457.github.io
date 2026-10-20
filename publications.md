---
layout: page
title: Publications
permalink: /publications/
---

Publications transcribed from my [Google Scholar bibliography](https://scholar.google.com/citations?user=dfY4ZvkAAAAJ&hl=en). Metadata reflects the supplied export; links are provided only where verified or explicitly available. [Download BibTeX]({{ '/assets/data/citations.bib' | relative_url }}).

<div class="pub-tools">
  <input id="pub-search" type="search" placeholder="Search titles, authors, venues…" aria-label="Search publications">
  <select id="pub-type" aria-label="Filter publication type"><option value="">All types</option><option>Journal article</option><option>Conference paper</option><option>Thesis</option></select>
  <span id="pub-count">{{ site.data.publications.size }} records</span>
</div>

{% assign years = site.data.publications | group_by: 'year' | sort: 'name' | reverse %}
{% for group in years %}
<section class="pub-year">
<h2>{{ group.name }}</h2>
{% for paper in group.items %}
<article class="pub-entry" data-type="{{ paper.type | escape }}" data-search="{{ paper.title | append: ' ' | append: paper.authors | append: ' ' | append: paper.venue | downcase | escape }}">
  <div class="pub-tag">{{ paper.type }}</div>
  <h3>{{ paper.title }}</h3>
  <p>{{ paper.authors }}</p>
  <p class="pub-venue"><em>{{ paper.venue }}</em>{% if paper.volume != '' %}, {{ paper.volume }}{% endif %}{% if paper.number != '' %}({{ paper.number }}){% endif %}{% if paper.pages != '' %}, {{ paper.pages }}{% endif %}.</p>
  {% if paper.doi != '' %}<a href="https://doi.org/{{ paper.doi }}">DOI</a>{% endif %}
  {% if paper.url != '' %}<a href="{{ paper.url }}">Publication</a>{% endif %}
</article>
{% endfor %}
</section>
{% endfor %}
<p id="pub-empty" hidden>No publications match these filters.</p>
<script>
(function () {
 const search = document.getElementById('pub-search');
 const type = document.getElementById('pub-type');
 const items = [...document.querySelectorAll('.pub-entry')];
 function update() {
  const q = search.value.toLowerCase().trim(); let shown=0;
  items.forEach(el => { const visible=el.dataset.search.includes(q) && (!type.value || el.dataset.type===type.value); el.hidden=!visible; if(visible)shown++; });
  document.querySelectorAll('.pub-year').forEach(el=>el.hidden=![...el.querySelectorAll('.pub-entry')].some(x=>!x.hidden));
  document.getElementById('pub-count').textContent=shown+' records';
  document.getElementById('pub-empty').hidden=shown!==0;
 }
 search.addEventListener('input',update);type.addEventListener('change',update);
})();
</script>
