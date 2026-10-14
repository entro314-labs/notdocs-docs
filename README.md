---
id: 01M4WNM3Z767X0BH4F7R6XK0QK
type: page
title: notdocs documentation
---

# notdocs documentation

The public documentation of [notdocs](https://notdocs.dev), kept as a notdocs project: markdown files in this repository, published by notdocs itself.

- `docs/` holds the pages. `docs/index.md` is the home; every other page is `docs/<page>.md` and is served at `<page>`.
- `notdocs.config.json` sets `"visibility": "public"`, so every page under `docs/` is on the published site. This README is outside the docs root and stays private.
- Pages link to each other by relative paths (`[Editor](editor.md#images)`), which resolve on the published site, on GitHub and in any markdown editor. Links to the rest of notdocs.dev are absolute (`https://notdocs.dev/pricing`).
- `title`, `description` and `order` in each page's frontmatter are the page's heading, its meta description and its place in the sidebar.

## Editing

Open the project in the notdocs workspace, or edit the files and push. Locally:

```sh
npm install -g notdocs
notdocs serve .            # the daemon on http://localhost:4400
notdocs lint               # tables, trees, diagrams and fences that render wrong
notdocs links              # links that reach nothing
```

## Publishing

The hosted organization connects this repository as a project; every push to `main` is served on the next visit. The site is at `https://<org>.notdocs.dev/<project>/p`, each page at `/<project>/p/<page>`, and its markdown at the same address with `.md`. See `docs/publishing.md` for how visibility and addresses work.
