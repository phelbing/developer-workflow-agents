# Template for the project CLAUDE.md

A plugin cannot ship a `CLAUDE.md` that is loaded automatically. Copy this block into your own `CLAUDE.md`. It only points to the skills and does not repeat any rules, so they are maintained in one place. After pasting, number the headings to fit the numbering of your own `CLAUDE.md`.

---

## Development

1. Shared rules (approach, quality, review, git, database queries, security): skill `developer-workflow-agents:engineering-conventions`.
2. Delegation to subagents and standard workflow: skill `developer-workflow-agents:delegation-routing`.
3. Code review by the review agent of the stack plugin: before every commit on the diff of that commit, before every PR on all commits of the PR.

## Project-specific (fill in)

1. Docker service name: `...`
2. Test command: `...`
3. Static analysis / code style: `...`
