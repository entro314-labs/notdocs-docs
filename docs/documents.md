---
id: 01M33YRKXCETFJE2ZKB0FZQDFA
type: page
title: Documents
description: "Markdown with frontmatter: ids, types, templates and custom types."
order: 10
updated: 2026-09-30
---

## Anatomy

A document is markdown with YAML frontmatter. Two keys matter:

- `id`, a ULID that notdocs stamps on a file the first time it sees it, so links survive a rename.
- `type`, which picks the schema: `page`, `task`, `decision`, `spec`, `meeting`, `launch`, or a custom type from `notdocs.config.json`.

```markdown title="docs/tasks/login.md" {2,3}
---
id: 01K57Q3M8ZJ0S4V9W2X7Y1AB3C
type: task
title: Add login
status: doing
priority: high
---

Email and password first, passkeys after.
```

Frontmatter shows in the workspace as properties; the body is the editor. The editor saves through the same markdown pipeline the CLI and MCP use, so a no-op edit is a zero-line diff.

## File kinds

A document is a `.md` file, and also an `.mdd`, an `.mdc` or an `.mdx`:

- `.mdd` is an [MDD](business-documents.md) document, the same dialect with its directives, read and saved exactly like `.md`.
- `.mdc` is markdown with frontmatter, most often Cursor's rules. Those live in `.cursor/`, so they are a coding agent's files and read only with `agentFiles` on; an `.mdc` anywhere else is read like `.md`.
- `.mdx` is read-only. It is listed, indexed, searched, linked and shown, its JSX and `import` and `export` lines as written and never run, but notdocs does not save it: the markdown serializer would escape those lines and its `{expressions}` and turn an indented JSX child into a code block. The editor opens it read-only with a note saying so, and `docs.update`, `docs.create` and `notdocs docs edit` refuse it; `notdocs lint` passes it by. Edit it in your editor. Like any document it gets an `id` in its frontmatter the first time notdocs indexes it; that edit leaves the body's bytes alone.

New documents are `.md`. A move or rename keeps a document's extension, links and wikilinks find the other kinds the way they find `.md` (`[[offer]]` finds `offer.mdd`), and the sidebar marks a document that is not `.md` with its extension. `.markdown` files are not read.

## Editing together

A document open in more than one browser is one shared session: everybody's keystrokes and carets show as they are typed, and the server writes the file after a pause. [Editing together](editing-together.md) has the details and what it costs.

## Order in the sidebar

The sidebar lists folders first, then documents, each alphabetically. An `order` number in the frontmatter places a document among its siblings: the ones with an `order` come first, lowest first, and the rest follow alphabetically. A folder takes the order of its `index.md`, the page `notdocs docs mkdir` creates, so a folder without one stays alphabetical. **Move up** and **Move down** in a row's menu write the key; `notdocs docs set docs/guide.md order=3` does the same from the CLI, and an agent writes it like any other key. The order lives in the file, so every client sees the same one.

## The dialect

CommonMark and GFM with YAML frontmatter and a small set of directives: callouts such as `:::note`, embeds, and [query blocks](query-blocks.md). Obsidian-style callouts (`> [!note]`) are GFM alerts and render as callouts too. A fenced `mermaid` block is drawn as its diagram, a `tree` block as a file tree and a block of raw HTML as what it draws; the file keeps the source. [The editor](editor.md) describes each.

Markdown written elsewhere often misses these by a line. Settings › Tables, trees and diagrams, `notdocs lint` and the `markdown_check` MCP tool find a table GFM does not read (its delimiter row wider or narrower than the header, or missing), a file tree or Mermaid source outside a code block, and Mermaid in a fence with no language; Fix, `notdocs lint --fix` and `markdown_fix` repair them in one commit that changes only those lines. They also report a code block that holds the start of another, which makes every fence after it pair up wrong; that one is left to you. Pasting such text into a page makes the table, tree or diagram as it goes in.

## Links and backlinks

A document links to another with a `[[wikilink]]`, which names it by title, file name, path or an alias in its frontmatter, or with a markdown link to its path (`[the spec](../specs/billing.md)`). Each document's links are indexed as written, and the **Links** tab in the rail lists the documents that link to the one on screen; `notdocs docs backlinks <doc>` and the `docs_backlinks` MCP tool answer the same list. When a document is moved or renamed, the links to it in other documents are rewritten to its new path, and its own relative links and images are rewritten to still point where they did.

## Images

Paste an image into a page, drop it there, or pick one with `/image`; `notdocs assets upload <file>` and the `assets_upload` MCP tool do the same for an agent. Where it is stored depends on the deployment: see [Images](editor.md#images) on the editor page.

## Built-in types

::::tabs
:::tab[task]
```yaml
type: task
status: doing        # todo, doing, blocked, review, done
priority: p1         # p0 to p3
assignee: ada
area: billing
due: 2026-09-22
blockedBy: [docs/tasks/tier-names.md]
```
:::
:::tab[decision]
```yaml
type: decision
status: accepted     # proposed, accepted, rejected, superseded
date: 2026-09-15
supersedes: docs/decisions/seats.md
```
:::
:::tab[spec]
```yaml
type: spec
status: draft        # draft, review, approved, implemented
owner: kai
area: billing
```
:::
:::tab[meeting]
```yaml
type: meeting
date: 2026-09-16     # required
attendees: [ada, kai]
```
:::
:::tab[launch]
```yaml
type: launch
status: ready        # planning, ready, launched, retro
channel: changelog
date: 2026-09-22
```
:::
:::tab[brief]
```yaml
type: brief
status: draft        # draft, review, approved
audience: developers
```
:::
:::tab[persona]
```yaml
type: persona
segment: platform teams
```
:::
:::tab[experiment]
```yaml
type: experiment
status: running      # proposed, running, concluded
metric: activation
owner: ada
```
:::
:::tab[release]
```yaml
type: release
version: 1.4.0       # required
date: 2026-09-16     # required
```
:::
:::tab[runbook]
```yaml
type: runbook
service: api
owner: kai
```
:::
:::tab[slo]
```yaml
type: slo
service: api         # required
objective: 99.9%
window: 30d
```
:::
:::tab[postmortem]
```yaml
type: postmortem
date: 2026-09-16     # required
status: review       # draft, review, final
severity: sev2       # sev1 to sev4
service: api
```
:::
:::tab[playbook]
```yaml
type: playbook
audience: support
```
:::
::::

`page` has no required keys beyond `title`. Eleven more built-in types are business and legal documents: `letter`, `proposal`, `invoice`, `contract`, `memo`, `report`, `terms`, `privacy`, `nda`, `dpa` and `sla`, with MDD frontmatter and blocks; see [Business documents](business-documents.md). The schema is validated on create and on update; a write that breaks it is refused with the reason.

## Templates and custom types

Templates are keyed by name, and several can start documents of one type: `prd` and `rfc` are both specs, `adr` is a decision. `notdocs templates` lists them and the [Templates](templates.md) page maps them to the stage of a product where each is written. Files in `docs/_templates/` are the repository's own; their frontmatter `type` decides the schema they produce and a `description` is shown in the list, and one named like a built-in replaces it. `notdocs docs new docs/tasks/login.md --template task` starts from one, and the workspace's new-document dialog lists them.

Custom types are declared in `notdocs.config.json`, which also sets the docs root (default `docs/`, where new documents go; `""` is the whole repository), the root files that count as documents (`README.md`, `CHANGELOG.md`, `CLAUDE.md`, `AGENTS.md`, in whatever case the repository spells them, so `readme.md` counts), `include`: globs of markdown read where it is besides the docs root (`"projects/**"`), `agentFiles`: whether the files coding agents keep count as documents (instructions below the root such as `packages/api/CLAUDE.md`, anything in `.claude/`, `.cursor/` and the like, Copilot's prompts, `skills/<name>/` and a plugin's commands and skills; `false` unless set), paths to ignore, and `searchLanguage`: the language full-text search stems and drops stop words in (`english` unless set; `simple` stems nothing, for a workspace of mixed languages; or the name of any text search configuration Postgres ships, such as `german` or `french`).

```json title="notdocs.config.json"
{
  "docsRoot": "docs",
  "rootFiles": ["README.md", "CHANGELOG.md", "CLAUDE.md", "AGENTS.md"],
  "include": ["projects/**"],
  "types": [
    {
      "name": "vendor",
      "fields": [
        { "name": "contact", "kind": "string", "required": true },
        { "name": "risk", "kind": "enum", "values": ["low", "high"], "default": "low" },
        { "name": "owners", "kind": "string[]" }
      ],
      "template": "vendor"
    }
  ],
  "ignore": ["node_modules/**", "**/.git/**"]
}
```

Settings › Markdown elsewhere lists the markdown the workspace does not read, by folder, with the coding agents' files apart; an owner ticks the folders to read and they are added to `include`. `notdocs config survey` prints the same list. A document read through `include` stays where it is and can be edited, moved, searched and linked like any other, but the sidebar keeps it out of sight: it shows the docs root and the root files, and a Markdown elsewhere row counts the rest and shows it, in its folders, when you ask (each person's choice, kept per project). The palette, search, boards and the project home list it all along.

Organize (on that row, and in Settings › Markdown elsewhere) moves it under the docs root the notdocs way, each document keeping its path below the root: `projects/a.md` becomes `docs/projects/a.md`. It is one commit, in which each document keeps its id, history and comments and the links to and from it are rewritten; the include globs the move empties are dropped in a second commit, and the links are checked before and after, so what the move broke is told apart from what was already broken. The coding agents' files never move. `notdocs docs organize` shows the same plan and `--apply` carries it out. Once it is done, the dialog offers a review by an agent: Ask the assistant puts the prompt in the assistant's box, and the same text is the `review-organization` prompt of the MCP server. The agent fixes the links the move left broken, as edits you approve, and suggests types and folder names without changing them. An include glob never reads a vendored or built folder (`node_modules`, `vendor`, `dist`, `build`, …), a license, or a changelog below the root.

A field's `kind` is one of `string`, `number`, `boolean`, `date`, `enum` (with `values`) or `string[]`; `required` defaults to false and `default` fills a missing value. `template` names the template in `docs/_templates/` that new documents of the type start from.

:::note
Custom type names are lowercase letters, digits and hyphens, starting with a letter. A document whose `type` matches no schema is validated against the base schema only, which asks for a `title`.
:::
