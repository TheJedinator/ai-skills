# Obsidian CLI — full command reference

Every command with its parameters/flags. `obsidian help <command>` is authoritative
for the installed version if anything here is unclear. Values with spaces must be quoted.

## Table of contents

- [General](#general)
- [Files and folders](#files-and-folders)
- [Daily notes](#daily-notes)
- [Search](#search)
- [Tasks](#tasks)
- [Properties](#properties)
- [Tags](#tags)
- [Links](#links)
- [Outline](#outline)
- [Aliases](#aliases)
- [Bookmarks](#bookmarks)
- [File history](#file-history)
- [Sync](#sync)
- [Command palette & hotkeys](#command-palette--hotkeys)
- [Plugins](#plugins)
- [Themes and snippets](#themes-and-snippets)
- [Templates](#templates)
- [Bases](#bases)
- [Workspace & tabs](#workspace--tabs)
- [Vault](#vault)
- [Publish](#publish)
- [Random & unique notes](#random--unique-notes)
- [Web viewer](#web-viewer)
- [Word count](#word-count)
- [Developer commands](#developer-commands)

## General

- `help [<command>]` — list all commands, or show help for one command
- `version` — show Obsidian version
- `reload` — reload the app window
- `restart` — restart the app

## Files and folders

- `file` — info (path, name, extension, size, created, modified). Params: `file`, `path`
- `files` — list files. Params: `folder`, `ext`, `total`
- `folder` — Params: `path` (required), `info=files|folders|size`
- `folders` — Params: `folder`, `total`
- `open` — open a note. Params: `file`, `path`, `newtab`
- `create` — Params: `name`, `path`, `content`, `template`; flags: `overwrite`, `open`, `newtab`
- `read` — print note contents. Params: `file`, `path`
- `append` — Params: `file`, `path`, `content` (required), `inline`
- `prepend` — Params: `file`, `path`, `content` (required), `inline`
- `move` — Params: `file`, `path`, `to` (required). Updates internal links if enabled
- `rename` — Params: `file`, `path`, `name` (required). Extension preserved automatically
- `delete` — Params: `file`, `path`; flag `permanent` (skip trash)

## Daily notes

- `daily` — open today's daily note. Param: `paneType=tab|split|window`
- `daily:path` — print path to today's note
- `daily:read` — print today's note
- `daily:append` — Params: `content` (required), `paneType`, `inline`, `open`
- `daily:prepend` — same params as append

## Search

- `search` — Params: `query` (required), `path`, `limit`, `format=text|json`, `total`, `case`
- `search:context` — grep-style `path:line: text`. Params: `query` (required), `path`, `limit`, `format`, `case`
- `search:open` — open search in app. Param: `query`

## Tasks

- `tasks` — Params: `file`, `path`, `status`, `total`, `done`, `todo`, `verbose`, `format=json|tsv|csv`, `active`, `daily`
- `task` — Params: `ref=<path:line>`, `file`, `path`, `line`, `status`, `toggle`, `daily`, `done`, `todo`

## Properties

Frontmatter properties.

- `properties` — Params: `file`, `path`, `name`, `sort=count`, `format=yaml|json|tsv`, `total`, `counts`, `active`
- `property:set` — Params: `name` (required), `value` (required), `type=text|list|number|checkbox|date|datetime`, `file`, `path`
- `property:remove` — Params: `name` (required), `file`, `path`
- `property:read` — Params: `name` (required), `file`, `path`

## Tags

- `tags` — Params: `file`, `path`, `sort=count`, `total`, `counts`, `format=json|tsv|csv`, `active`
- `tag` — Params: `name` (required), `total`, `verbose`

## Links

- `backlinks` — Params: `file`, `path`, `counts`, `total`, `format=json|tsv|csv`
- `links` — Params: `file`, `path`, `total`
- `unresolved` — unresolved links. Params: `total`, `counts`, `verbose`, `format=json|tsv|csv`
- `orphans` — notes with no links. Param: `total`
- `deadends` — notes with no outgoing links. Param: `total`

## Outline

- `outline` — Params: `file`, `path`, `format=tree|md|json`, `total`

## Aliases

- `aliases` — Params: `file`, `path`, `total`, `verbose`, `active`

## Bookmarks

- `bookmarks` — Params: `total`, `verbose`, `format=json|tsv|csv`
- `bookmark` — Params: `file`, `subpath`, `folder`, `search`, `url`, `title`

## File history

Local + Sync version history. Versions numbered newest → oldest.

- `diff` — Params: `file`, `path`, `from=<n>`, `to=<n>`, `filter=local|sync`
- `history` — Params: `file`, `path`
- `history:list`
- `history:read` — Params: `file`, `path`, `version` (default 1)
- `history:restore` — Params: `file`, `path`, `version` (required)
- `history:open` — Params: `file`, `path`

## Sync

Controls Sync within the running app. For headless CLI sync, see Obsidian Headless.

- `sync` — flags: `on`, `off`
- `sync:status`
- `sync:history` — Params: `file`, `path`, `total`
- `sync:read` — Params: `file`, `path`, `version` (required)
- `sync:restore` — Params: `file`, `path`, `version` (required)
- `sync:open` — Params: `file`, `path`
- `sync:deleted` — Param: `total`

## Command palette & hotkeys

- `commands` — Param: `filter=<prefix>`
- `command` — run a command. Param: `id=<command-id>` (required)
- `hotkeys` — Params: `total`, `verbose`, `format=json|tsv|csv`
- `hotkey` — Params: `id=<command-id>` (required), `verbose`

## Plugins

- `plugins` — Params: `filter=core|community`, `versions`, `format=json|tsv|csv`
- `plugins:enabled` — same options
- `plugins:restrict` — flags: `on`, `off`
- `plugin` — info. Param: `id` (required)
- `plugin:enable` — Params: `id` (required), `filter=core|community`
- `plugin:disable` — Params: `id` (required), `filter=core|community`
- `plugin:install` — Params: `id` (required), `enable`
- `plugin:uninstall` — Param: `id` (required)
- `plugin:reload` — Param: `id` (required)

## Themes and snippets

- `themes` — Param: `versions`
- `theme` — info. Param: `name`
- `theme:set` — Param: `name` (required; empty for default)
- `theme:install` — Params: `name` (required), `enable`
- `theme:uninstall` — Param: `name` (required)
- `snippets`
- `snippets:enabled`
- `snippet:enable` — Param: `name` (required)
- `snippet:disable` — Param: `name` (required)

## Templates

- `templates` — Param: `total`
- `template:read` — Params: `name` (required), `title`, `resolve`
- `template:insert` — Param: `name` (required)
- `resolve` processes `{{date}}`, `{{time}}`, `{{title}}` variables.

## Bases

`.base` files.

- `bases` — list `.base` files
- `base:views` — list views in current base
- `base:create` — Params/flags: `file`, `path`, `view`, `name`, `content`, `open`, `newtab`
- `base:query` — Params: `file`, `path`, `view`, `format=json|csv|tsv|md|paths`

## Workspace & tabs

- `workspace` — Param: `ids`
- `workspaces` — Param: `total`
- `workspace:save` — Param: `name`
- `workspace:load` — Param: `name` (required)
- `workspace:delete` — Param: `name` (required)
- `tabs` — Param: `ids`
- `tab:open` — Params: `group`, `file`, `view`
- `recents` — Param: `total`

## Vault

- `vault` — Param: `info=name|path|files|folders|size`
- `vaults` — Params: `total`, `verbose`
- `vault:open` — Param: `name` (required). TUI only

## Publish

- `publish:site`
- `publish:list` — Param: `total`
- `publish:status` — Params: `total`, `new`, `changed`, `deleted`
- `publish:add` — Params: `file`, `path`, `changed`
- `publish:remove` — Params: `file`, `path`
- `publish:open` — Params: `file`, `path`

## Random & unique notes

- `random` — Params: `folder`, `newtab`
- `random:read` — Param: `folder`
- `unique` — Params: `name`, `content`, `paneType=tab|split|window`, `open`

## Web viewer

- `web` — Params: `url` (required), `newtab`

## Word count

- `wordcount` — Params: `file`, `path`, `words`, `characters`

## Developer commands

- `devtools` — toggle Electron dev tools
- `dev:debug` — flags: `on`, `off`
- `dev:cdp` — Params: `method` (required), `params`
- `dev:errors` — Param: `clear`
- `dev:screenshot` — Param: `path` (returns base64 PNG)
- `dev:console` — Params: `limit` (default 50), `level=log|warn|error|info|debug`, `clear`
- `dev:css` — Params: `selector` (required), `prop`
- `dev:dom` — Params: `selector` (required), `attr`, `css`, `total`, `text`, `inner`, `all`
- `dev:mobile` — flags: `on`, `off`
- `eval` — run JS against the app. Param: `code` (required). Example: `eval code="app.vault.getFiles().length"`
