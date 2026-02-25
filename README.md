# Algo Notes with GitHub Pages

This repo is configured as a Markdown-based site using Jekyll and GitHub Pages.

## Structure

- `_algorithms/` -> your algorithm notes
- `algorithms-list.md` -> auto-generated list page
- `index.md` -> homepage
- `_config.yml` -> site config
- `.github/workflows/pages.yml` -> automatic deployment

## Add a new algorithm note

Create a file in `_algorithms/`, for example:

`_algorithms/binary-search.md`

Use this template:

```md
---
title: Binary Search
difficulty: Easy
tags: [binary-search, array]
---

## Problem
...

## Key idea
...

## Complexity
- Time: O(log n)
- Space: O(1)
```

It will appear automatically on `/algorithms-list/`.

## Deploy on GitHub Pages

1. Create a GitHub repo and push this project to `main`.
2. In GitHub: `Settings -> Pages`.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.
4. Push changes to `main`; the workflow deploys automatically.
5. Your site URL will be:
   `https://<your-username>.github.io/<repo-name>/`

## Optional local preview

If you want local preview:

```bash
bundle exec jekyll serve
```
