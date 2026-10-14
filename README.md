# AI Skills Plugin Marketplace

A Claude Code plugin marketplace providing skills that extend Claude's capabilities with specialized knowledge and workflows.

## Available Plugins

| Plugin | What it does |
|---|---|
| babysit-pr-stack | Owns a chain of stacked PRs until every PR merges: merge order, restacking, reviews, CI |
| creating-pull-requests | Creates a GitHub PR with a `## Why` / `## What` body from the branch diff |
| handoff | Summarizes the current session inline for another agent or person |
| linear-issues | Queries and creates Linear issues, sprints, triage and team workload via GraphQL |
| mattpocock-skills | Matt Pocock's skills (grilling, to-spec, tdd, code-review and more), installed from [mattpocock/skills](https://github.com/mattpocock/skills). Not copied here; updates come from upstream |
| stacked-prs | Creates, cascade-rebases, merges down and restructures stacked PR chains |
| tidy-first | Separates structural (tidying) commits from behavioral commits |

## Matt Pocock's skills

`mattpocock-skills` points at his repository (MIT licence, © Matt Pocock). It installs all of his skills, because his repo is a single plugin. After installing, run `/setup-matt-pocock-skills` once per repo. His `to-spec` and `to-tickets` skills need it.

## Installation

### 1. Add the Marketplace

```bash
/plugin marketplace add TheJedinator/ai-skills
```

### 2. Install a Plugin

```bash
# Install for all projects (user scope)
/plugin install stacked-prs@ai-skills

# Or install for current project only
claude plugin install stacked-prs@ai-skills --scope project
```

## Plugin Structure

This marketplace follows the Claude Code plugin format:

```
ai-skills/
├── .claude-plugin/
│   └── marketplace.json        # Marketplace manifest
└── linear-issues/              # Plugin
    ├── .claude-plugin/
    │   └── plugin.json         # Plugin manifest
    └── skills/
        └── linear-issues/
            ├── SKILL.md        # Main skill file
            └── references/     # On-demand reference material
```

## Contributing

To add a new plugin:

1. Create a plugin directory at the root (e.g., `my-plugin/`)
2. Add `.claude-plugin/plugin.json` with name, version, description
3. Add your skills under `skills/` within the plugin
4. Update `marketplace.json` to include your plugin
5. Submit a PR

## License

MIT License - see [LICENSE](LICENSE)
