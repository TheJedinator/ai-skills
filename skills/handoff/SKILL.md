---
name: handoff
description: Summarize the current session inline for handing off to another agent or person - what was done, what was learned, what comes next. Use when the user says "handoff", "/handoff", "hand this off", "summarize the session", "write a handoff", or "brief the next agent". Output is inline chat only, never written to a file.
---

# Handoff

Print the summary in chat. Do not create or edit any file.

Three sections, in this order. Bullets, one fact per line.

**Did** — changes made this session. Name the file paths. Name the commands run
and whether they passed.

**Learned** — facts about the codebase or system discovered this session. Each
line must trace to a file you read, a command whose output you saw, or code you
ran. Cite it: `path/to/file.py:42`, or the command.

**Next** — the specific next action, and what it is blocked on if anything.

## Rules

- Anything you did not read in a file or prove by running something does not go
  in. Not as a guess, not as a "likely", not as a flagged assumption. Omit it.
- No hedging: no "probably", "seems to", "might be worth", "consider", "you may
  want to".
- No caveats, no disclaimers, no "note that this is not exhaustive".
- No suggestions the user didn't ask for. `Next` is what the work requires, not
  ideas for improvements.
- Unknown and load-bearing → one line under `Next`: `Unknown: X. Check by: Y.`
- No preamble, no closing offer to help. Start at the first section header.
- Cut any section with nothing verified in it. An empty `Learned` is a fact.
