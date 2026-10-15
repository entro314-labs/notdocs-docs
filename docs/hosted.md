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

:::note[Who a commit is by]
notdocs is the committer of every commit it makes; the author is whoever asked for the change, so the repository's own history says who changed what. A person's commits carry their name and email address. A commit an app connected over OAuth makes for a person (Claude, say) is authored by that person, with a `Via: <app>` trailer in its message naming the app. A commit made with a project token is authored by the token's name as `<name>@agent`. notdocs' own writes, such as stamping an `id` on a new file, carry no author of their own.
:::

## Sharing without an account

A guest needs an account. Anyone else can read a document you share by link, or a project's published site: see [Publishing](publishing.md) for the levels, the addresses and a domain of your own.

## Plans

An organization is on Free, Solo, Team or Enterprise; the [pricing page](https://notdocs.dev/pricing) lists what each allows. The plan caps people and projects, sets the upload size, turns semantic search on from Solo, includes a monthly assistant allowance from Solo, publishes sites from Solo, and opens custom domains on Team. One request to the API carries about 3 MB on the hosted service, whatever the plan: an image pasted or dropped in the web app goes straight to the store, so it is bound by the plan alone, while a Word import and an upload from the CLI or an agent are bound by the request as well, and the Plan section says both. An owner changes it from the Plan section of a project's settings or with `notdocs org upgrade`; payment goes through Polar and the plan switches as soon as Polar confirms. When something is not in the plan, the API answers with `plan_limit` and a sentence naming the plan that would allow it.

## Agents on a hosted workspace

Create a project token in settings or with the CLI, then connect over streamable HTTP or link a checkout. The [agents page](https://notdocs.dev/agents) has the commands for Claude Code and Cursor.

```sh title="From a checkout"
notdocs link --url https://acme.notdocs.dev --project handbook
notdocs status
```

A write token can be limited to folders, so an agent that keeps the changelog cannot rewrite the handbook:

```sh
notdocs token create --name "changelog bot" --path docs/changelog --path notes
```

It still reads the whole project, but writes only documents, comments and images under those folders, given as paths from the repository root and matched without regard to case. A move must start and end inside them, and is refused when a document outside them links to the moved one, since that link would be rewritten in the same commit. Operations that write wherever the project needs (`markdown.fix`, `index.rebuild`, `docs.moveMany`) are refused to such a token. In the settings the same limit is "Folders it may write". Its commits are authored as `changelog bot@agent`, as above.
