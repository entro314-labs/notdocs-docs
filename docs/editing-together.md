---
id: 01M33YRMFNVJY210PPS2PEHWX4
type: page
title: Editing together
description: "A document open in several browsers is one session with everybody's carets; the server writes the file after a pause."
order: 12
updated: 2026-09-22
---

## One session per open document

A document open in more than one browser is one shared session. Every keystroke, the title and the properties show on every screen as they are typed, other people's carets show in their colour with their name beside them, and the top bar shows who else has the document open. Two tabs of your own count too: a document open twice is one session, not two copies waiting to collide.

There is nothing to turn on. Open a document somebody else has open and you are in its session; close it and you have left.

## The file stays the truth

The server holds the session and is the only one writing the file. It writes after a short pause in the typing: 1.5 seconds of quiet, 10 seconds at most while people keep typing, and when the last person leaves. ⌘S writes now. Each write is an ordinary save, so the History, the change feed and an agent reading `changes.since` see one change per pause rather than one per keystroke, and on the hosted service each is a commit like any other.

:::note[Why the server writes]
A browser can close mid-sentence. The server keeps the editor's schema and its markdown serializer (the `@notdocs/editor` package the web editor is built from) and writes what the session holds, so keystrokes are never stranded in a tab. The price is a window of at most 1.5 seconds of typing that a crash of the server process loses.
:::

## When the file changes underneath

A change can land on disk while people type: an agent writing over MCP, a push to the branch, a checkout on the local daemon. The server folds it into the session with the same three-way merge a save uses. Where the change touches other lines than the session, both survive and nobody has to do anything. Where the two collide, the session's version is kept and written, the page says so, and the file's version stays in the History, so nothing is lost either way.

## Where sessions run

| Deployment | Sessions |
|---|---|
| Local daemon | On, held in the daemon process |
| Self-hosted image | On: `docker compose` ships the K/V store; a single container without one sets `NOTDOCS_SESSIONS=process` |
| Hosted | On: the instances share a session through the K/V store |
| Anything else without a K/V store | Off; the editor works alone and edits are merged when they are saved, as before |

Sessions travel down over Server-Sent Events and up by POST, with the same bearer as every other call, so the standard Next server the self-hosted image runs serves them without a WebSocket. The same routes serve every deployment.

## What it costs

- While a session is on, the [Source view](editor.md#read-edit-source) shows the markdown but does not edit it; leave the session (close the other tabs) to edit the text as source.
- At most 1.5 seconds of typing can be lost if the server process crashes between two writes.
- The hosted service needs its K/V store; without one the API says sessions are off and the editor works alone.

## Compared with a merge on save

A session is for people in the same document at the same moment. Everything else, an agent's write, a push, a colleague on another branch, still meets the document as a save with a base hash, and is three-way merged as [Branches and merges](branches.md) describes. The two are the same merge, run at different moments.

::::cards
:::card[The editor]{href="editor.md" icon="pen"}
The toolbar, Read and Source, and the blocks that draw.
:::
:::card[Branches and merges]{href="branches.md" icon="branch"}
What happens when a save meets another save.
:::
:::card[Local]{href="local.md" icon="folder"}
The daemon on your machine, sessions included.
:::
::::
