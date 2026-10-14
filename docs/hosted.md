---
id: 01M36B607Y9D4G8X12TDH55WK3
type: page
title: Hosted
description: "Sign in, install the GitHub App, pick a repository, and every save is a commit."
order: 1
updated: 2026-09-16
---

## Set up

::::steps
:::step[Sign in and create an organization]
It gets a subdomain, `acme.notdocs.dev`. Organizations have owners and members; owners administer people, tokens, GitHub and domains.
:::
:::step[Install the notdocs GitHub App]
Install it on the GitHub account that owns your repositories. GitHub sends you back to attach the installation to your organization. Your notdocs account needs a GitHub connection for this step (sign in with GitHub, or add GitHub to your account in your profile): the installation is attached only if that GitHub account can reach it. An organization can only register repositories of the account it connected.
:::
:::step[Pick a repository]
It becomes a project at `acme.notdocs.dev/<project>`. Or choose **Create docs repo**, which makes a private `<login>/<slug>-docs` repository seeded with the starter files and registers it as the "Docs" project.
:::
::::

## How writes work

Every save is a commit on the branch you are viewing, made by the GitHub App. Pushes from anywhere else arrive through a webhook and re-index the changed files, so a commit from your editor or from CI shows up in the workspace without anything else to do.

:::warning[Commits carry the app as committer]
The GitHub App commits as `notdocs`. Who made the change is recorded in the workspace's history and change feed, not in the commit's author field.
:::

## Sharing without an account

A guest needs an account. Anyone else can read a document you share by link or publish:

- **Private**: only the people in the workspace.
- **Anyone with the link**: the document has an address of its own, `acme.notdocs.dev/<project>/p/<id>`. It is not listed on the site and search engines are told not to index it.
- **Public**: the document is on the project's published site, `acme.notdocs.dev/<project>/p/<path>`, with the other published documents in its sidebar, and in the sitemap.

The setting is `visibility` in a document's frontmatter, so it is a commit like any other change and agents see it. A document without one takes it from the nearest folder whose `index.md` sets one, then from the project's default (`visibility` in `notdocs.config.json`), then from the organization's, which starts private. The project and organization defaults cover the documents under the docs root; the root files (README, AGENTS.md and the rest) and markdown read through include globs are published only by a setting of their own or of their folder. Templates are never published.

- On a document, the globe button sets it and shows the address to send.
- On a folder, "Who can read this…" in the sidebar menu sets it on the folder's `index.md`, and creates that file when the folder has none. The docs folder's own index speaks for every document under it.
- The project's and the organization's defaults are under **Publishing** in the settings.
- From a terminal, `notdocs publish <path> <public|link|private|inherit>`, `notdocs publish --project-default <level>` and `--org-default <level>`; without a level it prints who can read the document and where.

Visitors read the default branch. A link in a published document to a document they may not read keeps its words and loses its address, images are served from the documents that show them, a query block lists the published documents it finds, and an embedded `![[image.png]]` shows the image. Comments, history and properties are never shown. A change shows on the next visit. Free shares by link; Solo and above publish a site, and on Free a document set to public is shared by link.

Each published page is also the markdown file it is: add `.md` to its address (`index.md` for the home), or send an `Accept` header that prefers `text/markdown`, and the file comes back as written, frontmatter included. The site answers `llms.txt` under the project's base (`/<project>/p/llms.txt` on the subdomain, `/<project>/llms.txt` on a custom domain) with every listed page by the address of its markdown, and a search box in its header searches the listed pages by words, through `/api/public/search?org=<org>&project=<project>&q=<words>`, which answers JSON for an agent too. A visitor's search never uses embeddings. A browser that offers WebMCP gets the site's pages, a page's markdown and the search as read-only tools for its own agent.

## Your own domain

An organization's published documents can be served on a domain you own, `docs.acme.com/<project>/<path>`, set from the organization's settings or with `notdocs org domain set`. The settings show the DNS records to create and poll verification. The domain serves the published site, with its own sitemap; the workspace and sign-in stay on the organization's subdomain.

## Plans

An organization is on Free, Solo, Team or Enterprise; the [pricing page](https://notdocs.dev/pricing) lists what each allows. The plan caps people and projects, sets the upload size, turns semantic search on from Solo, includes a monthly assistant allowance from Solo, publishes sites from Solo, and opens custom domains on Team. One request to the API carries about 3 MB on the hosted service, whatever the plan: an image pasted or dropped in the web app goes straight to the store, so it is bound by the plan alone, while a Word import and an upload from the CLI or an agent are bound by the request as well, and the Plan section says both. An owner changes it from the Plan section of a project's settings or with `notdocs org upgrade`; payment goes through Polar and the plan switches as soon as Polar confirms. When something is not in the plan, the API answers with `plan_limit` and a sentence naming the plan that would allow it. A self-hosted instance has no plans: everything is unlimited.

## Agents on a hosted workspace

Create a project token in settings or with the CLI, then connect over streamable HTTP or link a checkout. The [agents page](https://notdocs.dev/agents) has the commands for Claude Code and Cursor.

```sh title="From a checkout"
notdocs link --url https://acme.notdocs.dev --project handbook
notdocs status
```
