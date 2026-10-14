---
id: 01M4WNEPZJ84PR1GF1KSPF820C
type: page
title: Comments
description: "Threads that live in the repository next to the documents they are about."
order: 14
updated: 2026-09-16
---

## Where they live

Comments are files in the repository, in `.notdocs/comments/<docId>.yaml`, committed like everything else. A thread is anchored to a heading slug, a block, or `doc` for the whole document, and keeps a quote of the text at anchor time so it survives edits; the file is keyed by the document's id, so it survives a rename too.

```yaml title=".notdocs/comments/01K57Q3M8ZJ0S4V9W2X7Y1AB3C.yaml"
docId: 01K57Q3M8ZJ0S4V9W2X7Y1AB3C
threads:
  - id: 01K57R1V3NQ8P6M2X4Y9Z0AB7C
    anchor: enterprise-row
    quote: the enterprise row links to a form
    resolved: true
    comments:
      - id: 01K57R1V3NQ8P6M2X4Y9Z0AB7D
        author: ada@acme.dev
        body: Do we already have a form for this, or is it new?
        createdAt: 2026-09-16T09:12:00.000Z
      - id: 01K57R2B8WT4K1H7V3N5Q9ZC2E
        author: claude-code@agent
        body: Sales has one at /contact/sales. I linked it in the spec.
        createdAt: 2026-09-16T09:14:30.000Z
```

## In the workspace

Comment handles sit in the gutter beside headings (⌘⇧M starts a thread on the heading the cursor is in). The comments rail (⌘⇧C) lists threads, replies and resolves them. Resolving is optimistic: the thread closes at once and reopens with a note if the write fails.

## From the CLI and for agents

```sh
notdocs comments docs/launches/pricing.md --unresolved
notdocs comment docs/launches/pricing.md "Is the annual price final?" --anchor pricing
notdocs reply docs/launches/pricing.md 01K57R1V3NQ8P6M2X4Y9Z0AB7C "Yes, two months free."
notdocs resolve docs/launches/pricing.md 01K57R1V3NQ8P6M2X4Y9Z0AB7C
```

The same four operations, `comments.list`, `comments.add`, `comments.reply` and `comments.resolve`, are tools for an agent, and the **Review comments** prompt lists the unresolved threads on a document and acts on them.

:::warning[Authors are email addresses]
A comment's `author` is the writer's email, or `<token name>@agent` for an agent. Everyone who can read the repository can read them.
:::
