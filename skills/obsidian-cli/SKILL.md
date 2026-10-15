---
name: obsidian-cli
description: >-
  Drive Obsidian from the terminal via the `obsidian` CLI (Obsidian 1.12+):
  read/create/append/search notes, manage daily notes, tasks, tags,
  properties/frontmatter, links/backlinks, bookmarks, plugins, themes,
  workspaces, and Sync. Use whenever the user wants to interact with their
  Obsidian vault, notes, or knowledge base from the command line or a script —
  e.g. "add this to my daily note", "search my Obsidian vault for X", "create
  a note", "append a task to today", "read my note about Y", "list my vault's
  tags" — even if they don't say "CLI". Prefer this over guessing shell/file
  edits against the vault, since it resolves wikilinks, updates internal links,
  and keeps the app in sync.
---

# Obsidian CLI

The `obsidian` command controls a running Obsidian app from the terminal. Use it
instead of editing vault files directly — it resolves wikilinks, keeps internal
links updated on move/rename, and reflects changes in the live app.

## Before running commands

- **Requires the CLI enabled** (Settings → General → Command line interface).
  Verify with `obsidian version`. If the command is missing or the setting is
  absent, the installed Obsidian predates the CLI; tell the user to update the app.
- **The app must be running.** The first command will launch it if not; expect a
  brief delay. If commands hang or error with no app, tell the user to open Obsidian.
- macOS only: if `obsidian` isn't found, the symlink may be missing:
  `sudo ln -sf /Applications/Obsidian.app/Contents/MacOS/obsidian-cli /usr/local/bin/obsidian`

## Syntax

```
obsidian <command> [param=value ...] [flag ...]
```

- **Parameters** take a value: `content="- [ ] Buy milk"`. Quote values with spaces.
- **Flags** are bare boolean switches: `open`, `newtab`, `overwrite`, `permanent`.
- **Multiline:** use `\n` for newline, `\t` for tab inside a quoted value.
- **`obsidian help`** lists all commands; **`obsidian help <command>`** shows one command's params.
- **`--copy`** appended to any command copies its output to the clipboard.

### Targeting a vault and file

- **Vault** (optional, must be the *first* param): `vault=<name>` or `vault=<id>`.
  Defaults to the vault matching the current working directory, or the active vault.
- **File**: `file=<name>` resolves like a wikilink (no path needed) — prefer this.
  `path=<path>` is an exact path from the vault root. Defaults to the active file if omitted.

## Common workflows

**Daily notes** — the most frequent ask ("add to today", "log this"):
```
obsidian daily                                   # open today's daily note
obsidian daily:read                              # print today's note
obsidian daily:append content="- [ ] Buy groceries"
obsidian daily:prepend content="## Morning" open
```

**Read / search:**
```
obsidian read file=Recipe                        # print a note (active note if omitted)
obsidian search query="meeting notes" limit=10
obsidian search:context query="TODO"             # grep-style path:line: text
obsidian search:open query="budget"              # open search in the app
```

**Create / edit notes:**
```
obsidian create name="Trip to Paris" template=Travel open
obsidian create name="Idea" content="First line\nSecond line" newtab
obsidian append file=Journal content="- new entry"
obsidian prepend file=Journal content="top line"
obsidian move file=Draft to="Archive/Draft"      # updates internal links
obsidian rename file=Draft name="Final"          # keeps extension
obsidian delete file=Old                          # add `permanent` to skip trash
```

**Tasks:**
```
obsidian tasks daily                             # tasks in today's daily note
obsidian tasks file=Project todo format=json
obsidian task ref="Recipe.md:8" toggle           # toggle a task at path:line
```

**Frontmatter / properties:**
```
obsidian property:set name=status value=done type=text file=Note
obsidian property:read name=status file=Note
obsidian property:remove name=draft file=Note
```

## Full command reference

The list above covers the most common needs. For anything else — links/backlinks,
tags, bookmarks, file history & diffs, plugins, themes, snippets, workspaces,
publish, bases, Sync, word count, or developer commands (`eval`, `dev:*`) — read
`references/commands.md`, which catalogs every command and its params.

When you know roughly what you want but not the exact params, `obsidian help <command>`
is authoritative for the installed version — prefer it over guessing.

## Notes on output and scripting

- Many list/read commands accept `format=json|tsv|csv` (and some `md`, `yaml`,
  `paths`, `tree`). Use `format=json` when you'll parse the output programmatically.
- Most list commands accept `total` to print just a count, and many accept
  `counts` to include per-item counts.
- `eval code="..."` runs arbitrary JS against the app's `app` object — powerful
  but use sparingly and only when a dedicated command doesn't exist.
