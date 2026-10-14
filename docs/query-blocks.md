---
id: 01M33YRKXACAG8R8ZAD2FD4R6T
type: page
title: Query blocks
description: "A fenced block that renders as a board, a table or a list of documents."
order: 13
updated: 2026-09-22
---

## A fence that renders

A fenced block with the `notdocs` language is a query over the documents on the branch. It renders where it sits, as a board, a table, a list, a calendar or a gallery.

````markdown
```notdocs
type: task
where: { status: doing }
view: board
groupBy: status
```
````

## Keys

| Key | Values | What it does |
|---|---|---|
| `type` | a document type | Only documents of this type |
| `where` | a map of frontmatter key to value | Only documents whose frontmatter matches every entry |
| `sort` | a frontmatter key, or `-key` | Order; a leading minus sorts descending |
| `view` | `table`, `board`, `list`, `calendar`, `gallery` | How the result renders; `table` is the default |
| `groupBy` | a frontmatter key | Columns of a board, or sections of a gallery |
| `dateKey` | a frontmatter key | The key a calendar places a document by; `date`, then `due`, when unset |
| `limit` | a number | At most this many documents |

## Views

::::tabs
:::tab[board]
One column per value of `groupBy`, usually `status`. Moving a card writes the new value into that document's frontmatter and commits it; ⌥← and ⌥→ move the focused card without a pointer.
:::
:::tab[table]
One row per document with the frontmatter keys as columns. The first column stays put while the table scrolls, columns resize and sort, and rows can be selected.
:::
:::tab[list]
One row per document with its title, type and status, narrowed by status chips.
:::
:::tab[calendar]
A month grid with each document on the day its `date` or `due` names, or the key `dateKey` points at. It opens on the current month, or on the month with the most documents when this one has none; the documents without a date are counted under the grid.
:::
:::tab[gallery]
One card per document: its `cover` image when the frontmatter names one (a path in the repository or a URL), the title, up to two lines of `description` or `summary`, and the status. With `groupBy`, one section per value.
:::
::::

## Where they run

The web app renders a query block in place. `notdocs query '<yaml>'` prints the same groups in the terminal, and the `query.run` operation is a tool for agents, so a board on a page and a board in an agent's context are the same answer.

:::tip
The project's home page is itself a query over tasks, switchable between board, table and list, with changes per day drawn above it.
:::
