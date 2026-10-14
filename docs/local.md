---
id: 01M4WNEPZEKS29H5TW54TRXN5Q
type: page
title: Local
description: "Install the notdocs CLI from npm and run the daemon on your machine, against any folder of markdown, offline."
order: 2
updated: 2026-10-06
---

## Install and run

The CLI and the daemon are the `notdocs` package on npm, one bundle with the workspace packages inlined.

```sh
npm install -g notdocs
notdocs init ./my-project      # notdocs.config.json, docs/, agent instructions
notdocs serve ./my-project     # http://localhost:4400: API, change feed, index
```

`notdocs init` writes the repo config, a `docs/_templates/` folder, a roadmap page and two starter tasks, and appends a short section to your `CLAUDE.md` or `AGENTS.md` so an agent knows the workspace is there.

:::files
- my-project/
  - docs/
    - _templates/
      - README.md how templates work
    - tasks/
      - write-the-first-spec.md
      - connect-an-agent.md
    - roadmap.md
  - .notdocs/
    - index/ the daemon's index
  - notdocs.config.json
  - CLAUDE.md with a notdocs section appended
:::

## What the daemon does

`notdocs serve` runs the HTTP API, the websocket change feed and the index for one folder. It watches the folder, so edits from any editor show up in the workspace, and it keeps its index in `.notdocs/index` inside the folder, so it works offline and needs no account. The index holds the project and its change feed: a daemon started again on the same folder and data directory is the same project, at the same address, with its History. Edits made while it was stopped are found when it starts and recorded then, one entry per changed document; the commits made in between are not read.

| Option | Default | Meaning |
|---|---|---|
| `--port <port>` | `4400` | Where the API listens |
| `--data-dir <dir>` | `<folder>/.notdocs/index` | Where the index is persisted |

## Pointing at a hosted workspace instead

`notdocs link` writes a git-ignored `.notdocs/link.json` so every command, `notdocs mcp` included, talks to a hosted workspace from that checkout. The token stays in `NOTDOCS_TOKEN` unless you pass `--token`. `notdocs status` prints where commands point and how each value was chosen; see the [CLI reference](https://notdocs.dev/cli).

:::tip[Agents on a local folder]
`notdocs mcp --serve ./my-project` runs an in-memory daemon and the MCP server in one process, which is what `claude mcp add notdocs -- notdocs mcp --serve ./my-project` starts.
:::
