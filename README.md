# Deepak Kumar Panda — academic website

Free GitHub Pages website powered by Jekyll / Minima.

## Publications

`_data/publications.yml` contains **27** records parsed from the uploaded Google Scholar BibTeX export (`assets/data/citations.bib`). The file includes 14 journal articles, 12 conference papers and 1 thesis. Dates and publication metadata are preserved from the supplied export; verify any unusual online-first years and missing DOIs before publication.

## Deploy

1. Create a **public** GitHub repository named `YOURUSERNAME.github.io`.
2. Upload the **contents** of this directory to the repository root.
3. Set `url: "https://YOURUSERNAME.github.io"` in `_config.yml` (leave `baseurl: ""`).
4. In Settings → Pages choose **GitHub Actions**.
5. Commit to `main` and watch the Pages deployment workflow.

## Write a blog post

Create `_posts/YYYY-MM-DD-title.md`:

```markdown
---
layout: post
title: "My technical article"
date: 2026-11-01
categories: [cybersecurity, autonomy]
---

Your Markdown text and code here.

Inline math: $x^2$.
```

For mathematical typesetting add MathJax or KaTeX; the starter template does not automatically load a math renderer. For images, store them in `assets/images/` and reference them with Jekyll's `relative_url` filter.

## Local preview

Install Ruby/Bundler, then run `bundle install` and `bundle exec jekyll serve`.

## Notes

The publications are based solely on the uploaded export, not an independently validated scrape of Google Scholar. No missing DOI or paper URL has been invented.
