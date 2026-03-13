# Agent Docs — Maintainer Guide

This folder holds project knowledge written for AI agent consumption. It complements `AGENTS.md` (which defines workflow) with deeper reference material that agents load on demand.

## Why this exists

- **AGENTS.md** is always loaded and must stay concise. It defines *how agents work*.
- **Agent docs** are loaded selectively and can go deeper. They define *what agents need to know* about your project.

This separation keeps agent context windows lean while still giving them access to detailed project knowledge when needed.

## When to create a doc

Create a new doc when:

- You find yourself repeatedly explaining the same project convention to agents
- An agent makes a mistake that project-specific knowledge would have prevented
- A section of AGENTS.md is growing too long and only applies to specific tasks

Do NOT create a doc for:

- General programming knowledge agents already have
- Information that changes so frequently it can't be maintained
- One-off instructions (put those in the task prompt instead)

## How to add a doc

1. Create a markdown file in this folder (e.g., `api.md`, `database.md`).
2. Start with a title and a one-line purpose statement.
3. Write concise, actionable content — rules, patterns, and examples.
4. Register it in `index.md` by adding a row to the docs table with a meaningful "when to read" trigger.

## Writing tips

Write for agents, not humans. That means:

- **Lead with rules, not rationale.** "Always use parameterized queries" is better than a paragraph explaining why SQL injection is bad.
- **Use examples liberally.** A code snippet showing the right pattern is worth more than a paragraph describing it.
- **State the boundaries.** "This applies to files in `src/api/`" is more useful than leaving scope implicit.
- **Keep it current.** A wrong doc is worse than no doc. When you change a convention, update the doc or delete it.

## Structure

Start flat — just files in this folder. If you accumulate 10+ docs, consider grouping into subdirectories and updating `index.md` accordingly.

```
.agents/docs/
  index.md          # Always read first — lists all docs with triggers
  README.md         # This file (for human maintainers)
  architecture.md   # Example: system architecture overview
  api.md            # Example: API conventions and patterns
  ...
```
