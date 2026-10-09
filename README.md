# developer-workflow-agents

10 subagents with model routing (haiku, sonnet, opus) for a developer's workflow: planning, debugging, refactoring, documentation, GitHub issues, pull requests and release notes. Plus the shared engineering conventions. Independent of language and stack, and the base for `php-agents` and `symfony-agents` (separate repos).

## Installation

```bash
claude plugin marketplace add <owner>/developer-workflow-agents
claude plugin install developer-workflow-agents@developer-workflow-agents
```

Then run `/agents` in Claude Code. The agents carry the plugin prefix, e.g. `developer-workflow-agents:planner`.

## Contents

| Model | Agents |
|---|---|
| opus | planner, debugger |
| sonnet | refactorer, task-splitter, issue-writer, issue-implementer, release-notes |
| haiku | doc-writer, issue-triager, pr-writer |

Skills:
- `engineering-conventions`: shared rules for approach, quality, review, git, security and report format.
- `delegation-routing`: routing table, standard workflow (planner → task-splitter → issue-writer → issue-implementer → code review → pr-writer) and rules for handing over context.

## Structure of the conventions

Every rule exists exactly once, on the most general level it applies to. Higher levels load the lower ones and only add to them.

| Level | Plugin | Skill |
|---|---|---|
| Base (all stacks) | developer-workflow-agents | `engineering-conventions` |
| PHP | php-agents | `php-conventions`, `php-security-checklist`, `php-design-patterns` |
| Symfony | symfony-agents | `symfony-conventions`, `symfony-security-checklist` |

The agents load their skills at startup through the `skills` field in the frontmatter, including all levels below. In the main conversation, a skill loads the level below through the Skill tool. If a skill cannot be loaded, the agents say so in their report.

## Recommended additions to your project

A plugin does not load a `CLAUDE.md` or permissions into the project. Both are provided as templates under `examples/`:

- `examples/CLAUDE.template.md`: block for your own `CLAUDE.md`. Only points to the skills.
- `examples/settings.json`: permissions for `.claude/settings.json`. Allows `git` and `gh` to the extent needed, blocks force push, push to `main`/`master`, `ssh`/`scp`/`rsync` and reading `.env*`.

## Notes

- Do not set `CLAUDE_CODE_SUBAGENT_MODEL`, otherwise the variable overrides the `model:` lines of all agents.
- To change an agent's model: the `model:` line in the frontmatter under `agents/`.
- Read third-party agents before using them and check their tool permissions.
- `gh` must be logged in (`gh auth status`).
- The `deny` rules in `examples/settings.json` match the command as written. `git push` without a target on a checked-out `main`, `git push origin HEAD:main` or `rm -fr` are not covered. The rules guard against mistakes, they are not a hard block.

## For maintainers

```bash
claude plugin validate ./
```

`version` in `.claude-plugin/plugin.json` is set. Users stay on this version until you raise it. Dependencies without a version range follow the current state of the other plugins. For fixed version ranges, tag the releases with `claude plugin tag --push` and add the ranges to `dependencies`.

## License

MIT, see `LICENSE`.
