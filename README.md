# AI Skills

A collection of skills for Claude Code that extend its capabilities with specialized knowledge and workflows.

## What are Skills?

Skills are modular markdown packages that teach Claude Code domain-specific patterns, workflows, and best practices. They require zero infrastructure - just drop a skill folder into your `.claude/skills/` directory.

## Available Skills

### [django-models](skills/django-models/)

Navigate and understand Django models in any codebase. Provides:

- Model discovery patterns using Grep/Glob/Read
- Field types quick reference
- Relationship navigation strategies
- Common query patterns
- Financial domain business logic (accounting, payments, banking, FX)

## Installation

### Global Installation (all projects)

```bash
# Clone the repo
git clone https://github.com/TheJedinator/ai-skills.git

# Copy skill(s) to your Claude skills directory
cp -r ai-skills/skills/django-models ~/.claude/skills/
```

### Project-Specific Installation

Reference the skill in your project's `CLAUDE.md`:

```markdown
## Django Model Navigation

When exploring Django models, read the skill guide at `path/to/skills/django-models/SKILL.md`.
```

## Skill Structure

Each skill follows this structure:

```
skill-name/
├── SKILL.md              # Main skill file with frontmatter and instructions
└── references/           # Optional detailed reference material
    └── *.md              # Loaded on-demand by Claude
```

## Contributing

To add a new skill:

1. Create a folder under `skills/` with your skill name
2. Add a `SKILL.md` with YAML frontmatter (`name` and `description` fields)
3. Keep instructions concise - Claude is smart, only add what it doesn't already know
4. Add reference files for detailed domain knowledge that should load on-demand

## License

MIT License - see [LICENSE](LICENSE)
