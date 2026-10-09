---
name: engineering-conventions
description: Use at the start of any coding, review, refactoring, debugging or git task, and whenever another skill or agent refers to the shared engineering conventions. Stack-independent rules for scope, testing, git, secrets, review and reporting.
---

# Shared engineering conventions

Base for all agents and stacks. Stack-specific skills (e.g. `php-agents:php-conventions`, `symfony-agents:symfony-conventions`, `react-agents:react-conventions`) add to these rules and do not repeat them.

## 1. Approach

1. Name the problem first, then choose the solution. The smallest solution that solves it.
2. Read the affected files and the neighboring code. Project conventions take precedence.
3. Small diff, no changes outside the task. Report side issues you discover, do not fix them.
4. Unclear or contradictory: ask, do not guess. State your assumptions. As a subagent: stop the work and put the question in your report.

## 2. Quality

1. Run tests, static analysis and code style checks before finishing. Report the result as it is.
2. New behavior needs a test. A test that cannot turn red is worthless.
3. Review before every commit, on the diff of that commit. Review before every PR, on all commits of the PR. Migrations and changes to payment, login and permissions also need a review of the business logic.

## 3. Review

### 3.1 Scope

What the task names. If not stated:

1. Uncommitted changes present: `git diff HEAD` (staged and unstaged), plus new files from `git status --porcelain` (`??`).
2. Otherwise the branch against its base: `git diff <base>...HEAD`, base via `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`.

### 3.2 Order of checks

Correctness, security, data access (N+1, transactions, migrations), error handling, tests, design (unnecessary abstractions, hidden dependencies), style.

### 3.3 Report

One numbered entry per finding, sorted by severity (Blocker, Should, Nit):

```text
<n>. **<severity>** – `file:line` – problem – suggestion
```

Report only findings in new or changed code, or caused by it. A defect in existing code counts only when the change triggers it or makes it worse.

A finding that rests on an unchecked assumption about a tool, library or platform is verified first or marked as an assumption.

Only real findings, no praise, no repetition of the diff. No findings: "No objections" and one sentence on what was checked.

## 4. Git

1. Commit and push only when the task names it. Without that, the work stays uncommitted and the report says so.
2. Never push to `main`/`master`, no force push, no `reset --hard`.
3. One branch per task.
4. Branch name: `<type>/<nr>-<description>`, type by kind of work (`feature/`, `fix/`, `chore/`, `docs/`, `refactor/`). Without an issue: `<type>/<description>`. Description in English, even when the issue title is not.
5. One matter per commit. Check what belongs together before committing: a dependency update, a configuration change and a changelog entry go into separate commits.
6. Commit message in English, short and to the point. The code shows the details.
7. Do not delete issues, branches or repositories.
8. Only reference issues (`#<nr>`), without a keyword such as `Closes`, `Fixes` or `Resolves`. In a commit, the issue number goes at the end of the subject line (`Add role checks to API controllers #514`), not in the body.

## 5. Database queries

1. Before a query runs, check whether an index serves it: the table's index list or `EXPLAIN`.
2. Without a usable index the query carries a `WHERE`, a `LIMIT` or both. A query without either is the last resort, only for a table known to be small.
3. This holds for counts too: `COUNT(*)` on a large table reads every row.
4. On a busy database, ask the cheapest question: a few sampled rows instead of an aggregate over everything.

## 6. Security and environment

1. Do not read, print or commit secrets (`.env*`, keys, tokens). Name the location, never the value.
2. No access to production servers. Deployments are done by a human.
3. If development runs in Docker, run commands in the container: `docker compose exec <service> ...`. Check the service names in `docker-compose.yml`.
4. A service that was already running stays running: do not stop or restart it. A stack or dev server you started yourself is stopped again once the work is done. When in doubt, leave it running and say so in the report.
5. Never remove volumes (`docker compose down -v`).
6. `git` and `gh` run on the host, not in the container. A commit from the container carries the container's git identity.
7. Do not change files in `vendor/` and `node_modules/`.
8. No machine-specific paths in the repository (no `/home/<user>/…`, no private file outside the project), in code, configuration, comments and documentation alike. Use a path relative to the project, an environment variable or a placeholder such as `~/.ssh/<deploy-key>`.

## 7. Report

1. Result first, then evidence. Locations as `path:line`.
2. List open points and assumptions at the end.
