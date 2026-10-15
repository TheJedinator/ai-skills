# ai-skills

My agent skills, in one repo, for Claude Code, opencode, pi and Codex. This repo is the source of truth; my dotfiles symlink into it.

## Skills

Mine:

| Skill | What it does |
|---|---|
| babysit-pr-stack | Owns a chain of stacked PRs until every PR merges: merge order, restacking, reviews, CI |
| creating-pull-requests | Creates a GitHub PR with a `## Why` / `## What` body from the branch diff |
| handoff | Summarizes the current session inline for another agent or person |
| humanizer | Removes signs of AI writing from text, with voice samples to match my register |
| linear-issues | Queries and creates Linear issues, sprints, triage and team workload via GraphQL |
| obsidian-cli | Drives Obsidian from the terminal via the `obsidian` CLI |
| stacked-prs | Creates, cascade-rebases, merges down and restructures stacked PR chains |
| tidy-first | Separates structural (tidying) commits from behavioral commits |

From [mattpocock/skills](https://github.com/mattpocock/skills), copied so I can pick a subset and edit (see [CREDITS.md](CREDITS.md)):

| Skill | What it does |
|---|---|
| grilling, grill-me, grill-with-docs | Relentless interview to sharpen a plan; the docs variant also writes GLOSSARY.md and ADRs |
| to-spec, to-tickets | Turn a conversation into a spec, then into tracer-bullet tickets |
| implement, implement-spec | Build from a spec or tickets, driving tdd and closing with code-review |
| tdd | Red-green-refactor loop |
| code-review | Two-axis review: coding standards and spec fidelity |
| diagnosing-bugs | Disciplined diagnosis loop for hard bugs and regressions |
| prototype | Throwaway prototype to answer a design question |
| research | Investigate a question against primary sources into a cited Markdown file |
| domain-modeling, codebase-design | Domain vocabulary and deep-module design discipline |
| writing-for-agents | Writing skills, AGENTS.md and CLAUDE.md |
| setup-matt-pocock-skills | One-time per-repo setup for to-spec, to-tickets and friends |
| retro | Suggest environment improvements after a session |
| wizard | Generate a bash wizard for steps only a human can do |

## Install

Pick individual skills, any agent:

```bash
npx skills@latest add TheJedinator/ai-skills
```

Everything as one Claude Code plugin:

```bash
/plugin marketplace add TheJedinator/ai-skills
/plugin install ai-skills@ai-skills
```

On my machines, dotfiles symlink `~/.local/share/agent-skills/<skill>` to `skills/<skill>` in a clone at `~/personal-github/ai-skills`.

## Layout

```
.claude-plugin/
  marketplace.json   # one plugin, source ./
  plugin.json        # lists every skills/<name>
skills/<name>/
  SKILL.md
  ...                # references, agents, scripts
```

Add a skill: create `skills/<name>/SKILL.md`, add its path to `plugin.json`.

## Licence

MIT, see [LICENSE](LICENSE). Copied skills keep their own licence in `skills/LICENSE-mattpocock`.
