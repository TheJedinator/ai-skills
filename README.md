# AI Skills Plugin Marketplace

A Claude Code plugin marketplace providing skills that extend Claude's capabilities with specialized knowledge and workflows.

## Available Plugins

### django-models

Navigate and understand Django models in any codebase. Provides:

- Model discovery patterns using Grep/Glob/Read
- Field types quick reference
- Relationship navigation strategies
- Common query patterns
- Financial domain business logic (accounting, payments, banking, FX)

## Installation

### 1. Add the Marketplace

```bash
/plugin marketplace add TheJedinator/ai-skills
```

### 2. Install a Plugin

```bash
# Install for all projects (user scope)
/plugin install django-models@ai-skills

# Or install for current project only
claude plugin install django-models@ai-skills --scope project
```

## Plugin Structure

This marketplace follows the Claude Code plugin format:

```
ai-skills/
├── .claude-plugin/
│   └── marketplace.json        # Marketplace manifest
└── django-models/              # Plugin
    ├── .claude-plugin/
    │   └── plugin.json         # Plugin manifest
    └── skills/
        └── django-models/
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
