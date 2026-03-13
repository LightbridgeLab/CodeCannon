# AGENTS.md

Repository conventions for all AI coding agents (Claude Code, Copilot, Cursor, Windsurf, etc.).

This file defines a universal workflow for agent-assisted development with human oversight. Project-specific commands and settings are in `.agents/config.yaml`.

## Quick Start

1. Copy `AGENTS.md`, the `.agents/` folder (including `config.yaml`, `review-prompt.md`, and `docs/`) into your project root.
2. Edit `.agents/config.yaml` to match your project's branch names, commands, and preferences.
3. (Optional) Add the Makefile git targets from `Makefile.agents.mk` to your project's Makefile.
4. (Optional) Add project-specific agent docs to `.agents/docs/` — see the README there for guidance.
5. Point your agent's config file (e.g. `.claude/CLAUDE.md`) to this file: "Follow the workflow in AGENTS.md."

## Branch Strategy

Branch names and roles are configured in `.agents/config.yaml` under `workflow.branches`. The default strategy:

- **production branch** (e.g. `main`) — live deployment. **NEVER commit directly.**
- **integration branch** (e.g. `development`, `staging`, `dev`) — preview/staging. **NEVER push directly** — always use a PR.
- **feature branches** (e.g. `feature/*`) — short-lived. Branch FROM integration, PR back TO integration. Delete after merge.

### Merge path

```
feature/* --> integration --> production
```

There is no shortcut. Never open a PR from a feature branch directly to the production branch.

## Agent Task Checklist

When the user requests a code change, follow the three-phase workflow below. Each phase has explicit human gates — do not advance past a gate until the user confirms. (If `workflow.phases.human_gates` is `false` in config, gates are advisory only.)

### Phase 1: Discovery (no branch, no code changes)

Skip this phase if `workflow.phases.discovery` is `false` in config.

1. **Understand the request** — Read relevant code, ask clarifying questions.
2. **Propose an approach** — Explain what you'd change and why.
3. **HUMAN GATE** — Ask: "Does this approach sound right, or should we adjust?"
   - User says "yes" → proceed to Phase 2.
   - User redirects or explores → stay in Phase 1.
   - User abandons → done, nothing to clean up.

### Phase 2: Implementation (branch exists, code changes)

4. **Create a feature branch** — Run the `create_branch` command from config.
5. **Write the code** — Do NOT commit yet.
6. **HUMAN GATE** — Say: "The code is ready for review. Please test locally. Let me know if it looks good, needs changes, or should be scrapped."
   - User says "looks good" → commit the changes, proceed to Phase 3.
   - User requests changes → iterate, then re-test (repeat step 6).
   - User says "scrap it" → run the `abandon` command from config, done.

### Phase 3: Publish (PR, review, merge)

7. **Create a PR** — Run the `create_pr` command from config to push and open a PR targeting the integration branch.
8. **Spawn review agent** (if `workflow.phases.review_agent` is `true`) — Launch a code review agent using the prompt at `workflow.review.prompt` in config. The review agent posts its findings as a PR comment.
9. **Act on review verdict:**
   - If APPROVE with no critical findings → run the `merge_pr` command from config.
   - If REQUEST CHANGES → address findings (back to step 5), then re-review.
10. **Done.** Tell the user the PR is merged and what to do next (e.g. deploy).

### Exiting the workflow

- At any HUMAN GATE, the user may redirect, pause, or abandon. Respect this.
- If on a feature branch with uncommitted changes and the user wants to scrap: run the `abandon` command.
- Merges to the integration branch are permitted after review agent approval (or immediately if review is disabled). Merges to the production branch remain manual.

## Commit Hygiene

- Write meaningful commit messages in imperative mood ("Add X", "Fix Y", "Remove Z").
- Prefer small, focused commits over large ones.
- Do not commit: `.env` files, build artifacts, `node_modules`, or secrets of any kind.

## What Agents Must NEVER Do

- Push directly to the production or integration branches
- Merge PRs targeting the production branch (merges to integration are allowed after review)
- Delete the production or integration branches
- Bypass branch protection rules
- Run destructive commands (drop DB, clear prod data, etc.) without explicit human instruction

## Agent Reference Docs

For detailed project knowledge beyond what fits in this file, see `.agents/docs/index.md`. Read the index first; load individual docs only when relevant to your current task.

## Customization

### Adding project-specific sections

Add sections below this line for conventions specific to your project (coding style, architecture, file organization, etc.). When porting the workflow to another project, keep everything above and replace everything below.

---

<!-- Add your project-specific conventions below this line -->
