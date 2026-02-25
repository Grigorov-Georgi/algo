# Algo Notes (GitHub Pages)

Personal algorithm notes site powered by Jekyll + GitHub Pages.

## Repo layout

- `_algorithms/` - one Markdown page per algorithm
- `algorithms-list.md` - auto-generated list of all algorithms
- `index.md` - homepage
- `_config.yml` - Jekyll config and collection setup
- `.github/workflows/pages.yml` - automatic Pages deployment
- `ALGORITHM_PAGE_TEMPLATE.local.md` - local prompt/template for generating new pages (git-ignored)

## Algorithm page format

Each algorithm page should follow the structure in `ALGORITHM_PAGE_TEMPLATE.local.md` and include:

- clear Java 17 implementation
- short intuition
- when to use / not use
- complexity (best/avg/worst where relevant)
- pitfalls and edge cases
- worked example
- Java demo tests

## Add a new algorithm page

1. Create `_algorithms/<algorithm-slug>.md`
2. Use frontmatter:

```md
---
title: "Algorithm Name"
difficulty: Easy
tags: [tag1, tag2]
---
```

3. Fill the rest using the template structure.
4. Save and commit; it appears automatically on `/algorithms-list/`.

## Deploy

1. Push to `main`.
2. In GitHub go to `Settings -> Pages`.
3. Set **Source** to **GitHub Actions**.
4. Every push to `main` triggers deployment via `.github/workflows/pages.yml`.

Site URL:
`https://<your-username>.github.io/<repo-name>/`

## Optional local preview

```bash
bundle exec jekyll serve
```
