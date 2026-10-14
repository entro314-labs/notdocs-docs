---
id: 01M33YRKF9T76RZ5Z7XPCB1E10
type: page
title: Import
description: "Bring a Notion export, an Obsidian vault or Word documents into the repository as markdown."
order: 18
updated: 2026-09-23
---

## Three importers

`notdocs import notion` converts a Notion "Markdown & CSV" export, `notdocs import obsidian` converts a vault and `notdocs import docx` converts Word documents. The same importers run from the workspace's import page, with per-file progress rows. The first two take a `.zip` or a folder and have `--dry-run`, and store the images they carry the way pasted images are stored, pointing each page at them; all three stamp an `id` on every document they create. An `id` a note or a Notion row already has that is not a ULID (`id: intro`) is kept in `aliases`, so a wikilink by that name still finds the document. A file the workspace refuses is reported and the rest are still imported; the CLI then prints what was created, what already existed and what failed (as JSON with `--json`) and exits non-zero.

::::tabs
:::tab[Notion]
```sh
notdocs import notion ./Export-….zip --into docs/notion --dry-run
notdocs import notion ./Export-….zip --into docs/notion
```
Database rows become typed documents: a row whose database has a Status property becomes a `task`, everything else a `page`. Properties become frontmatter, fitted to the task's fields: Urgent, High, Medium and Low priorities become `p0` to `p3`, several people in a text field such as Assignee are joined, and a number or date is written as text; a value that still does not fit keeps its place as `notion<Name>` (`notionPriority: Someday`), and so does a Type property, since the importer sets `type`. The `Export-<uuid>/` wrapper and the page ids in file names and links are stripped, and two pages whose names are then the same become `Name.md` and `Name-2.md`, links included.
:::
:::tab[Obsidian]
```sh
notdocs import obsidian ~/Vaults/work --into docs --dry-run
notdocs import obsidian ~/Vaults/work --into docs
```
Wikilinks become relative markdown links resolved against the vault's file list, embeds become images or links, and callouts pass through untouched because they are already GFM alerts.
:::
:::tab[Word]
```sh
notdocs import docx ./Offer.docx ./Terms.docx --into docs/word
```
Each file becomes one page named after it, titled by its first heading. Headings, text, lists, tables and links are converted through [md-docx](https://github.com/markdownkit/md-docx); the pictures inside the file are stored the way pasted images are (in the repository under `assets/`, or in the object store on the hosted service) and the page shows them from there. An agent does the same with the `docs_import` MCP tool, which takes the file base64-encoded.
:::
::::

::::cards
:::card[Switch from Notion]{href="https://notdocs.dev/switch-from-notion" icon="upload"}
The export steps, a before-and-after, and what to do first afterwards.
:::
:::card[Switch from Obsidian]{href="https://notdocs.dev/switch-from-obsidian" icon="folder"}
What changes in a vault, and what stays exactly as it was.
:::
::::
