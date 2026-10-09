---
name: engineering-conventions
description: Use at the start of any coding, review, refactoring, debugging or git task, and whenever another skill or agent refers to the shared engineering conventions. Stack-independent rules for scope, testing, git, secrets, review and reporting.
---

# Shared engineering conventions

Base for all agents and stacks. Stack-specific skills (e.g. `php-agents:php-conventions`, `symfony-agents:symfony-conventions`) add to these rules and do not repeat them.

## Approach

1. Name the problem first, then choose the solution. The smallest solution that solves it.
2. Read the affected files and the neighboring code. Project conventions take precedence.
3. Small diff, no changes outside the task. Report side issues you discover, do not fix them.
4. Unclear or contradictory: ask, do not guess. State your assumptions. As a subagent: stop the work and put the question in your report.

## Quality

- Run tests, static analysis and code style checks before finishing. Report the result as it is.
- New behavior needs a test. A test that cannot turn red is worthless.
- Review before every commit or PR. Migrations and changes to payment, login and permissions also need a review of the business logic.

## Review

Scope: what the task names. If not stated:
- Uncommitted changes present: `git diff HEAD` (staged and unstaged), plus new files from `git status --porcelain` (`??`).
- Otherwise the branch against its base: `git diff <base>...HEAD`, base via `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`.

Order of checks: correctness, security, data access (N+1, transactions, migrations), error handling, tests, design (unnecessary abstractions, hidden dependencies), style.

Report, sorted by severity:
- **Blocker** – `file:line` – problem – suggestion
- **Should** – ...
- **Nit** – ...

Only real findings, no praise, no repetition of the diff. No findings: "No objections" and one sentence on what was checked.

## Git

- Never push to `main`/`master`, no force push, no `reset --hard`.
- One branch per task (`feature/<nr>-<slug>`, `fix/<nr>-<slug>`), small commits with meaningful messages.
- Do not delete issues, branches or repositories.
- Only reference issues (`#<nr>`), without a keyword such as `Closes`, `Fixes` or `Resolves`.

## Security and environment

- Do not read, print or commit secrets (`.env*`, keys, tokens). Name the location, never the value.
- No access to production servers. Deployments are done by a human.
- If development runs in Docker, run commands in the container: `docker compose exec <service> ...`. Check the service names in `docker-compose.yml`.
- `git` and `gh` run on the host, not in the container. A commit from the container carries the container's git identity.
- Do not change files in `vendor/` and `node_modules/`.

## Report

- Result first, then evidence. Locations as `path:line`.
- List open points and assumptions at the end.
