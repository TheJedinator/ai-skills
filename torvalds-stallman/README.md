# Torvalds-Stallman Code Reviewer

A sardonic, Socratic code reviewer for Claude Code that pressure-tests design decisions with the critical eye of a veteran principal engineer.

## Overview

Unlike standard code reviewers that focus on linting and test results, **torvalds-stallman** interrogates the decisions behind your code:

- **Naming & Readability** — Are names clear and honest about what they do?
- **Abstraction Level** — Over-engineered? Under-abstracted? YAGNI violations?
- **Logic Correctness** — Does the function do what its name promises?
- **Alternative Solutions** — Could a different approach be simpler or more idiomatic?
- **Performance** — N+1 queries, unnecessary iterations, premature optimization
- **Test Quality** — Mock depth, parameterization opportunities, redundant setup
- **Systems Thinking** — Downstream implications, coupling concerns

### Personality

Named after two of software's most notoriously unfiltered critics, this agent:

- Delivers **dry, sardonic** feedback with memorable edge
- Asks **Socratic questions** that force you to defend your choices
- **Scales criticism** with the offense (laziness gets roasted, premature abstraction gets mocked)
- **Stays silent on good code** — no false praise, just honest assessment

## Installation

### From Marketplace

```bash
claude install torvalds-stallman
```

### Manual Installation

1. Clone or download this plugin:
   ```bash
   git clone https://github.com/TheJedinator/ai-skills.git
   cd ai-skills/torvalds-stallman
   ```

2. Install into Claude Code:
   ```bash
   claude install .
   ```

## Usage

The agent triggers when you request code review with language suggesting you want honest, critical feedback:

### Example Requests

**Explicit review:**
```
"Review this code and tell me what you really think."
"Tear this apart. What's wrong with it?"
```

**Design decisions:**
```
"I'm not sure if these abstraction choices make sense. Can you critique the design?"
"Do these patterns make sense or am I over-engineering?"
```

**Thorough review:**
```
"Can you do a thorough review of this PR before I send it out?"
"I want to catch issues before my team sees this."
```

**Beyond linting:**
```
"The linter passes but something feels off. What's wrong?"
"What issues won't the automated tools catch?"
```

### Review Scope

The agent adapts to what you give it:

- **PR diffs** — Reviews changed code, skips boilerplate/migrations
- **Specific files** — Deep dive into a module or component
- **Functions/classes** — Focused critique on implementation
- **Architecture** — Design decisions and system implications

### What You'll Get

**Executive Summary**: Overall verdict and patterns observed

**Inline Findings**: Specific critiques with file:line references

**Socratic Questions**: Pointed inquiries about your choices

**No Rewrites**: Just critique. If you can't fix it from the feedback, that's a skill issue.

### What You Won't Get

- ❌ Formatting/style complaints (that's the linter's job)
- ❌ Confidence scores (not calibrated)
- ❌ Severity labels (unless categorically correct)
- ❌ Code rewrites (you implement, not the agent)

## When to Use

**Use torvalds-stallman when:**
- You want honest feedback on design decisions
- You're uncertain about abstraction choices
- You need critique beyond what linters catch
- You want to pressure-test your implementation before human review

**Use standard code-reviewer when:**
- You need lint/test/format compliance checks
- You want gentle, constructive feedback
- You're working with junior developers who need encouragement

## Configuration

No configuration needed. The agent uses the same model as your current Claude session.

## Examples

### Good Code (Silence)

When your code is clean and well-designed, the agent stays quiet:

```
Executive Summary: This is tight. Move on.
```

### Design Issues

When abstractions are questionable:

```
payment_processor.py:45 — Why is PaymentValidator a class with a single
static method? This is a function wearing a class costume. What's the
benefit over `def validate_payment()`?

order_service.py:78 — You're wrapping stripe.Customer.create() in a
CustomerFactory that does nothing except call it. This is indirection
for the sake of indirection. When does this factory become useful?
```

### Logic Bugs

When the code has subtle issues:

```
auth.py:23 — `get_user_or_none()` returns None on failure but raises
on invalid token format. Pick one error strategy. Right now callers
can't tell if they should catch exceptions or check for None.
```

## Development

This plugin contains only an agent — no commands, skills, or hooks.

**Structure:**
```
torvalds-stallman/
├── .claude-plugin/
│   └── plugin.json
├── agents/
│   └── torvalds-stallman.md
└── README.md
```

## License

MIT License - See repository for details

## Credits

Inspired by the brutally honest code review styles of Linus Torvalds and Richard Stallman.

Built with [Claude Code](https://code.claude.com).
