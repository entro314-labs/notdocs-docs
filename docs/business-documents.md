---
id: 01M4WNEPZJS1TH383394GVWMXT
type: page
title: Business documents
description: "Letters, proposals, invoices, contracts, memos, reports, terms, privacy policies, NDAs, DPAs and SLAs as MDD, exported to PDF and Word."
order: 19
updated: 2026-09-16
---

## Documents that leave the repository

Eleven built-in types are business documents: `letter`, `proposal`, `invoice`, `contract`, `memo` and `report`, and the legal set `terms`, `privacy`, `nda`, `dpa` and `sla`. They are ordinary notdocs documents, with properties, a place on boards, comments and history, and they are also [MDD](https://www.npmjs.com/package/@markdownkit/mdd) documents: the frontmatter carries MDD's `document-type`, `date` and `author`, and the body uses MDD's blocks for the parts a professional document has that a wiki page does not.

```markdown title="docs/letters/offer.md"
---
id: 01K57Q3M8ZJ0S4V9W2X7Y1AB3C
type: letter
document-type: business-letter
title: Offer
date: 2026-09-16
author: Ada Lovelace
recipient: Kai Tanaka
---

:::letterhead
Acme
1 Analytical Engine Way
London
:::

Dear Kai,

# Offer

We would like to work with you.

Sincerely,

:::signature-block
Ada Lovelace
Founder
:::
```

A file the MDD tools wrote carries `document-type` and no `type`. The first time notdocs sees it, it adds the `type` that `document-type` names (`business-letter` a letter, `terms-of-service` terms, `privacy-policy` privacy, the others their own name) and changes nothing else in the file. MDD has no DPA or SLA, so those carry its generic `agreement` and `service-agreement`; a file with one of those and no `type`, or with any other `document-type`, is a page.

## The blocks

| Block | What it is | Where it goes in Word and print |
|---|---|---|
| `:::letterhead` … `:::` | Who the document is from | The top of the first page |
| `:::header` … `:::` | A line repeated on every page; `{{page}}` is the page number | The page header |
| `:::footer` … `:::` | The same at the bottom | The page footer |
| `:::contact-info` … `:::` | How to reach the sender | A block in the flow |
| `:::signature-block` … `:::` | Who signs; the rule to sign on is drawn above it | A block that stays on one page |
| `::page-break` | Start a new page here | A page break |
| `::section-break` | A visible break between parts | A rule |

Blocks do not nest. Inside them, everything is markdown: bold, links, lists and tables all work, and `2^10^` and `H~2~O` are super- and subscripts.

:::note[MDD 3]
This is the MDD 3 spelling, which is the same directive grammar notdocs uses for callouts and query blocks, so a business document round-trips through the editor and the CLI byte for byte. A file written in the MDD 2 spelling (`::letterhead` … `::`) has its openers reported by validation; `mdd-migrate` from `@markdownkit/mdd` rewrites the file.
:::

## What each type asks for

::::tabs
:::tab[letter]
Requires `:::letterhead` and `:::signature-block`; recommends `:::contact-info`. Frontmatter: `recipient`, `subject`.
:::
:::tab[proposal]
Requires `:::letterhead`; recommends `:::header`, `:::footer` and `:::signature-block`. Frontmatter: `recipient`, `version`, `status` (`draft`, `sent`, `accepted`, `declined`), `reference-number`.
:::
:::tab[invoice]
Requires `:::letterhead` and `:::header`; recommends `:::footer` and `:::contact-info`. Frontmatter: `invoice-number` (required), `recipient`, `due-date`, `payment-terms`, `total-amount`, `purchase-order`.
:::
:::tab[contract]
Requires `:::letterhead` and `:::signature-block`; recommends `:::header` and `:::footer`. Frontmatter: `parties` (required, a list), `effective-date` (required), `expiration-date`, `jurisdiction`, `reference-number`, `version`.
:::
:::tab[memo]
Nothing required; recommends `:::header`. Frontmatter: `recipient`, `subject`.
:::
:::tab[report]
Nothing required; recommends `:::letterhead`, `:::header` and `:::footer`. Frontmatter: `version`, `status` (`draft`, `review`, `final`).
:::
:::tab[terms]
MDD `terms-of-service`. Nothing required; recommends `:::letterhead` and `:::footer`. Frontmatter: `effective-date` (required), `version`, `status`.
:::
:::tab[privacy]
MDD `privacy-policy`. Nothing required; recommends `:::letterhead` and `:::footer`. Frontmatter: `effective-date` (required), `version`, `status`.
:::
:::tab[nda]
Requires `:::signature-block`; recommends `:::letterhead`. Frontmatter: `parties` (required, a list), `effective-date` (required), `expiration-date`, `jurisdiction`, `status`.
:::
:::tab[dpa]
MDD `agreement`. Requires `:::letterhead` and `:::signature-block`; recommends `:::header` and `:::footer`. Frontmatter: `parties` (required, a list), `effective-date` (required), `jurisdiction`, `version`, `status`.
:::
:::tab[sla]
MDD `service-agreement`. Requires `:::letterhead` and `:::signature-block`; recommends `:::header` and `:::footer`. Frontmatter: `parties` (required, a list), `effective-date` (required), `jurisdiction`, `version`, `status`.
:::
::::

Every type takes `date` (required) and `author`. On the legal types `status` is one of MDD's own, `draft`, `pending`, `approved`, `final` or `archived`. A DPA is MDD's `agreement` and an SLA its `service-agreement`; the notdocs `type` is the shorter name, the way `letter` is MDD's `business-letter`. `docs.validate`, `notdocs validate` and the workspace check the frontmatter against the schema and the body against these requirements; a missing required block is an error, a missing recommended one a note.

## Templates

`notdocs docs new docs/letters/offer.md --template letter` starts from a template with the letterhead, the blocks the type needs and placeholder text to replace. Every type has a template of its name, and `competitive` and `research` are two more reports. The workspace's new-document dialog lists them all. A repository can replace any of them with its own file in `docs/_templates/`, which is where a real letterhead belongs.

## Export

::::tabs
:::tab[Workspace]
**Export** in the document's top bar. **Word document** downloads a `.docx` made by `@markdownkit/md-docx`, with the header and footer as real page furniture. **Print or save as PDF** opens the print preview: the document on A4 sheets as it prints, with its letterhead, header and footer, and a new sheet at each `::page-break`, in white whatever the app's theme. Its button opens the browser's print dialog on exactly that page, where **Save as PDF** makes the file. The command palette (⌘K) offers the same on an open document. Any document prints this way; one with no MDD blocks prints as plain pages with the same stylesheet.
:::
:::tab[CLI]
```sh
notdocs docs export docs/letters/offer.md --format docx
notdocs docs export docs/letters/offer.md --format html --out offer.html
```

The `.html` file is the page the preview shows: open it in a browser and print it to save a PDF. There is no `--format pdf`; asked for one, the command says to do this instead.
:::
:::tab[Agents]
`docs.export` takes the document id and `html` or `docx` and returns the file base64-encoded with its filename and content type. An agent can draft the letter, validate it and hand back the Word file in one turn.
:::
::::

On paper the header and footer move into the page margin and repeat on every page, with `{{page}}` as the page number; on screen, and in the preview, they show once, at the top and the end of the document. Chrome and Edge print them that way; Firefox and Safari cannot draw in the page margin yet and print the document without them, so print from Chrome or Edge when the header and footer matter.

:::tip[Also the mdd tools]
Because the file is valid MDD, `mdd-preview`, `mdd-validate` and `mdd-convert --format pdf` from `@markdownkit/mdd` work on it directly from a checkout.
:::
