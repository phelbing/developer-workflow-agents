# Template for the project CLAUDE.md

A plugin cannot ship a `CLAUDE.md` that is loaded automatically. Copy this block into your own `CLAUDE.md`. It only points to the skills and does not repeat any rules, so they are maintained in one place.

---

## Development

- Shared rules (approach, quality, review, git, database queries, security): skill `developer-workflow-agents:engineering-conventions`.
- Delegation to subagents and standard workflow: skill `developer-workflow-agents:delegation-routing`.
- Code review by the review agent of the stack plugin: before every commit on the diff of that commit, before every PR on all commits of the PR.

## Project-specific (fill in)

- Docker service name: `...`
- Test command: `...`
- Static analysis / code style: `...`
