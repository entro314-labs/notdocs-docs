---
id: 01M4WNEPZJ5QHKYPC5TTCPT2GW
type: page
title: Search
description: "Full text and embeddings over every document, for people and agents."
order: 16
updated: 2026-09-16
---

## Two indexes, one answer

Every document is indexed twice: its title and body for full-text search, and pieces of about 1,200 characters as embeddings for semantic search. A query runs both and merges the hits; the result says whether embeddings took part, and the palette marks such hits with a **semantic** badge.

## Where to search

::::tabs
:::tab[Palette]
⌘K opens the command palette. It searches document titles as you type and asks the server for full-text and semantic hits, alongside recent documents and quick actions.
:::
:::tab[CLI]
```sh
notdocs search "how do we bill yearly" --limit 5
```
:::
:::tab[Agents]
The `search` tool takes `q` and `limit` and returns hits with the document id, path, title and a snippet, plus `semantic: true` when embeddings were used.
:::
:::tab[Published sites]
A published site has a search box in its header, over its listed pages alone, and `/api/public/search?org=<org>&project=<project>&q=<words>` answers the same hits as JSON for an agent. A visitor's search is full-text only: a query from someone with no account is never embedded.
:::
::::

## Keeping it fresh

Hosted, every push re-indexes the changed files through the GitHub webhook. Locally, the daemon watches the folder. `notdocs reindex`, or **Rebuild index** in settings, stamps missing ids and rebuilds search and embeddings for the whole branch, with progress rows while it runs.

:::note[What leaves the service]
Embeddings are computed by a model through Vercel AI Gateway, which means document text is sent to that provider. The [AI disclosure](https://notdocs.dev/legal/ai) says exactly what, and how to have embeddings disabled for a project.
:::
