---
id: 01M4WNEPZJVB3XX3SGM4VYPCBV
type: page
title: Publishing
description: "Share a document by link or publish a site of them: visibility levels, addresses, a domain of your own, and the markdown, llms.txt and search a site serves to agents."
order: 17
updated: 2026-10-14
---

## Three levels

A guest needs an account. Anyone else can read a document you share by link or publish:

- **Private**: only the people in the workspace.
- **Anyone with the link** (`link`): the document has an address of its own, made of its id. It is not listed on the site, and search engines are told not to index it.
- **Public**: the document is on the project's published site, at an address made of its path, with the other published documents in its sidebar, in the site's search, in its `llms.txt` and in the sitemap.

## Where the setting lives

The setting is `visibility` in a document's frontmatter, so it is a commit like any other change, and agents and pull requests see it:

```markdown title="docs/guides/setup.md"
---
id: 01K57Q3M8ZJ0S4V9W2X7Y1AB3C
type: page
title: Setup
visibility: public
---
```

A document without one takes it from the nearest folder whose `index.md` sets one, then from the project's default (`visibility` in `notdocs.config.json`), then from the organization's, which starts private. A folder's index document speaks for the folder, so setting it on the docs root's `index.md` publishes everything under it.

```json title="notdocs.config.json"
{
  "visibility": "public"
}
```

The project and organization defaults cover the documents under the docs root only. The root files (README, AGENTS.md and the rest) and markdown read through `include` globs are published only by a setting of their own or of their folder, so an agent's instructions do not go public with the docs. Templates are never published, and visitors are served the default branch alone.

## Setting it

- On a document, the globe button sets it and shows the address to send.
- On a folder, "Who can read this…" in the sidebar menu sets it on the folder's `index.md`, and creates that file when the folder has none.
- The project's and the organization's defaults are under **Publishing** in the settings.
- From a terminal:

```sh
notdocs publish docs/guides/setup.md public   # one document
notdocs publish docs/guides link              # a folder, on its index.md
notdocs publish docs/guides/setup.md inherit  # drop its own setting
notdocs publish --project-default public      # notdocs.config.json
notdocs publish --org-default link            # the organization's default
notdocs publish docs/guides/setup.md          # who can read it, and where
```

Without a level, `notdocs publish` prints the level, where it came from (the document itself, a folder's index, the project or the organization) and the address. Agents use the same operations: `docs_visibility` reports, and `docs_update` sets the `visibility` key.

## Addresses

On the organization's subdomain, a project's site is under `p`:

- A listed document is at its path under the docs root, without the extension: `docs/guides/setup.md` is `acme.notdocs.dev/handbook/p/guides/setup`. A folder's `index.md` is the folder's address, and the docs root's `index.md` is the site's home, `acme.notdocs.dev/handbook/p`; without one, the home lists the pages.
- A document shared by link is at its id alone, `acme.notdocs.dev/handbook/p/01K57Q3M8ZJ0S4V9W2X7Y1AB3C`, since its path would let anyone who guesses it read it. A public document asked for by its id moves to its path.

The subdomain is also the workspace, so the rest of it still asks people to sign in.

## A domain of your own

An organization's published documents can be served on a domain you own, set from the organization's settings or with `notdocs org domain set docs.acme.com`; the settings and `notdocs org domain status` show the DNS records to create and whether the domain is verified. The domain serves the published site and nothing else: a project's pages are at `docs.acme.com/<project>/<path>`, without the `p`, its root lists the organization's published projects (or opens the one site when there is one), and it has its own `robots.txt` and sitemap. The workspace and sign-in stay on the organization's subdomain.

Once the domain is verified, it is where a page's canonical address and the sitemap point. A domain under notdocs.dev itself is refused, since the organization already has its subdomain there. A custom domain is part of the Team plan and above; on a plan without one it is kept, unserved, and serves again when the plan includes one.

## What a visitor sees

A visitor reads as a guest of the documents served to them. A link in a published document to a document they may not read keeps its words and loses its address; images are served from the documents that show them; a query block lists the published documents it finds; an embedded `![[image.png]]` shows the image. Comments, history and properties are never shown. A change shows on the next visit, and visitors are limited per address, since they have no credential.

## For agents

A published site is for machines as much as for readers, and an agent reads it without a credential, seeing exactly what a visitor sees:

- **Markdown**: every page is also the file it is, frontmatter included. Add `.md` to its address (`index.md` for the home, `<id>.md` for a document shared by link), or send an `Accept` header that prefers `text/markdown`. Each page links to its markdown, and its head names it as an alternate.
- **llms.txt**: under the project's base (`/<project>/p/llms.txt` on the subdomain, `/<project>/llms.txt` on a custom domain), listing every listed page by the address of its markdown.
- **Search**: the search box in the site's header calls `/api/public/search?org=<org>&project=<project>&q=<words>`, which answers JSON to an agent too. It covers the listed pages, by words; a visitor's search never uses embeddings.
- **WebMCP**: a browser that offers WebMCP gets three read-only tools on every page, for its own agent: `notdocs_site_pages`, `notdocs_site_page` and `notdocs_site_search`.

A document shared by link is served at `<id>.md` to whoever holds the id, and never listed or found. Writing needs a token or a connected app, as on the [agents page](https://notdocs.dev/agents).

## Plans

Free shares by link: a document set to public is reachable at its id and never listed or indexed. Solo and above publish a site. Team and above serve it on a domain of your own. A self-hosted instance has no plans, and the local daemon publishes nothing. The [pricing page](https://notdocs.dev/pricing) lists the rest.

This documentation is itself a notdocs project published this way: markdown files in a git repository, with `"visibility": "public"` in its `notdocs.config.json`.
