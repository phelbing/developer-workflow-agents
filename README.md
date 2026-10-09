# developer-workflow-agents

10 subagents with model routing (haiku, sonnet, opus) for a developer's workflow: planning, debugging, refactoring, documentation, GitHub issues, pull requests and release notes. Plus the shared engineering conventions. Independent of language and stack, and the base for [`php-agents`](https://github.com/phelbing/php-agents), [`symfony-agents`](https://github.com/phelbing/symfony-agents) and [`react-agents`](https://github.com/phelbing/react-agents) (separate repos).

## 1. Installation

```bash
claude plugin marketplace add phelbing/developer-workflow-agents
claude plugin install developer-workflow-agents@developer-workflow-agents
```

Then run `/agents` in Claude Code. The agents carry the plugin prefix, e.g. `developer-workflow-agents:planner`.

## 2. Contents

| Model | Agents |
|---|---|
| opus | planner, debugger |
| sonnet | refactorer, task-splitter, issue-writer, issue-implementer, release-notes |
| haiku | doc-writer, issue-triager, pr-writer |

Skills:
1. `engineering-conventions`: shared rules for approach, quality, review, git, database queries, security and report format.
2. `delegation-routing`: routing table, standard workflow (planner → task-splitter → issue-writer → issue-implementer → code review → pr-writer) and rules for handing over context.

## 3. Recommended additions to your project

A plugin does not load a `CLAUDE.md` or permissions into the project. Both are provided as templates under `examples/`:

1. `examples/CLAUDE.template.md`: block for your own `CLAUDE.md`. Only points to the skills.
2. `examples/settings.json`: permissions for `.claude/settings.json`. Allows `git` and `gh` to the extent needed, blocks force push, push to `main`/`master`, `ssh`/`scp`/`rsync` and reading `.env*`.

## 4. Notes

1. Do not set `CLAUDE_CODE_SUBAGENT_MODEL`, otherwise the variable overrides the `model:` lines of all agents.
2. Read third-party agents before using them and check their tool permissions.
3. `gh` must be logged in (`gh auth status`).
4. The `deny` rules in `examples/settings.json` match the command as written. `git push` without a target on a checked-out `main`, `git push origin HEAD:main` or `rm -fr` are not covered. The rules guard against mistakes, they are not a hard block.

## 5. Documentation

1. [docs/architecture.md](docs/architecture.md): how the conventions are layered and how the agents load their skills.
2. [docs/maintenance.md](docs/maintenance.md): validation, agent models and versions, for maintainers.

## 6. Related plugins

1. [`php-agents`](https://github.com/phelbing/php-agents): PHP and Doctrine agents for code search, tests, review, migrations, security and performance. Builds on developer-workflow-agents.
2. [`symfony-agents`](https://github.com/phelbing/symfony-agents): Symfony agents for implementation, tests, review and security. Builds on php-agents.
3. [`react-agents`](https://github.com/phelbing/react-agents): React, TypeScript and Vite agents for implementation, tests, review and test runs. Builds on developer-workflow-agents.

## 7. License

MIT, see `LICENSE`.
