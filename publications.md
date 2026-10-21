---
layout: page
title: Publications
permalink: /publications/
<section class="publications-intro">
  <p class="publications-eyebrow">ACADEMIC RESEARCH</p>
  <h1>Publications</h1>
  <p class="publications-description">Research in cybersecurity, trustworthy artificial intelligence, autonomous systems, reinforcement learning, and cyber-physical systems.</p>
  <div class="publications-profile-links">
    <a href="https://scholar.google.com/citations?user=dfY4ZvkAAAAJ" target="_blank" rel="noopener noreferrer">Google Scholar ↗</a>
    <a href="https://github.com/dp457" target="_blank" rel="noopener noreferrer">GitHub ↗</a>
  </div>
</section>
<div class="pub-tools" role="search" aria-label="Filter publications">
  <label for="pub-search">Search publications</label>
  <input id="pub-search" type="search" placeholder="Title, author, venue, keyword…" autocomplete="off">
  <label for="pub-year-filter">Year</label>
  <select id="pub-year-filter"><option value="all">All years</option>{% assign pub_years = site.data.publications | map: 'year' | uniq | sort | reverse %}{% for year in pub_years %}<option value="{{ year }}">{{ year }}</option>{% endfor %}</select>
  <label for="pub-type-filter">Type</label>
  <select id="pub-type-filter"><option value="all">All types</option><option value="Journal article">Journal articles</option><option value="Conference paper">Conference papers</option><option value="Thesis">Thesis</option></select>
  <button id="pub-clear" type="button">Clear filters</button>
</div>
<p id="pub-count" aria-live="polite"></p>
{% assign sorted_pubs = site.data.publications | sort: 'year' | reverse %}
<div id="pub-list">
{% for pub in sorted_pubs %}
<article class="pub-entry" data-year="{{ pub.year }}" data-type="{{ pub.type | escape }}" data-search="{{ pub.title | append: ' ' | append: pub.authors | append: ' ' | append: pub.venue | append: ' ' | append: pub.year | downcase | escape }}">
  <div class="pub-meta"><span class="pub-tag">{{ pub.type | escape }}</span><span class="pub-year">{{ pub.year }}</span></div>
  <h2>{{ pub.title | escape }}</h2>
  <p class="pub-authors">{{ pub.authors | escape }}</p>
  <p class="pub-venue">{{ pub.venue | escape }}{% if pub.volume != blank %}, vol. {{ pub.volume | escape }}{% endif %}{% if pub.number != blank %}, no. {{ pub.number | escape }}{% endif %}{% if pub.pages != blank %}, {{ pub.pages | escape }}{% endif %}</p>
  <div class="pub-actions">
    {% if pub.url != blank %}<a href="{{ pub.url | escape }}" target="_blank" rel="noopener noreferrer">Paper ↗</a>{% endif %}
    {% if pub.doi != blank %}<a href="https://doi.org/{{ pub.doi | escape }}" target="_blank" rel="noopener noreferrer">DOI ↗</a>{% endif %}
    <button type="button" class="pub-bibtex" data-id="{{ pub.id | escape }}" data-title="{{ pub.title | escape }}" data-authors="{{ pub.authors | escape }}" data-year="{{ pub.year }}" data-venue="{{ pub.venue | escape }}" data-type="{{ pub.type | escape }}" data-doi="{{ pub.doi | escape }}">Copy BibTeX</button>
  </div>
</article>
{% endfor %}
</div>
<p id="pub-empty" hidden>No publications match these filters.</p>
<script>
(function () {
  const search = document.getElementById('pub-search');
  const year = document.getElementById('pub-year-filter');
  const type = document.getElementById('pub-type-filter');
  const clear = document.getElementById('pub-clear');
  const entries = Array.from(document.querySelectorAll('.pub-entry'));
  const count = document.getElementById('pub-count');
  const empty = document.getElementById('pub-empty');
  function filter() {
    const q = search.value.trim().toLocaleLowerCase();
    let shown = 0;
    entries.forEach(function (entry) {
      const visible = (!q || entry.dataset.search.includes(q)) &&
        (year.value === 'all' || entry.dataset.year === year.value) &&
        (type.value === 'all' || entry.dataset.type === type.value);
      entry.hidden = !visible;
      if (visible) shown++;
    });
    count.textContent = 'Showing ' + shown + ' of ' + entries.length + ' publications';
    empty.hidden = shown !== 0;
  }
  [search, year, type].forEach(function (el) { el.addEventListener('input', filter); });
  clear.addEventListener('click', function () { search.value = ''; year.value = 'all'; type.value = 'all'; filter(); search.focus(); });
  filter();
  document.querySelectorAll('.pub-bibtex').forEach(function (button) {
    button.addEventListener('click', async function () {
      const d = button.dataset;
      const kind = d.type === 'Journal article' ? 'article' : d.type === 'Conference paper' ? 'inproceedings' : 'phdthesis';
      const author = d.authors.split(/,\s*/).reduce(function (acc, part, i, all) {
        if (i % 2 === 0) acc.push(part + (all[i + 1] ? ', ' + all[i + 1] : ''));
        return acc;
      }, []).join(' and ');
      const fields = ['  title = {' + d.title + '}', '  author = {' + author + '}', '  year = {' + d.year + '}'];
      fields.push('  ' + (kind === 'article' ? 'journal' : kind === 'inproceedings' ? 'booktitle' : 'school') + ' = {' + d.venue + '}');
      if (d.doi) fields.push('  doi = {' + d.doi + '}');
      const bib = '@' + kind + '{' + d.id + ',\n' + fields.join(',\n') + '\n}';
      try {
        await navigator.clipboard.writeText(bib);
        const old = button.textContent;
        button.textContent = 'Copied!';
        setTimeout(function () { button.textContent = old; }, 1600);
      } catch (e) { window.prompt('Copy BibTeX:', bib); }
    });
  });
})();
