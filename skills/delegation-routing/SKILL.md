---
name: delegation-routing
description: Use when delegating planning, debugging, refactoring, documentation, GitHub issue, pull request or release work to subagents, or when choosing which agent fits a task. Contains the routing table, the standard workflow and rules for handing over context.
---

# Delegation (workflow)

Delegate by decision complexity, not by type of task. The model is set in the frontmatter of each agent. Shared rules: skill `developer-workflow-agents:engineering-conventions`.

Call agents with the plugin prefix, e.g. `developer-workflow-agents:planner`.

| Task | Agent |
|---|---|
| Unclear design, architecture, trade-offs | planner |
| Bug without an obvious cause | debugger |
| Restructuring without behavior change | refactorer |
| Split plans into tasks | task-splitter |
| Draft issues, create them on request | issue-writer |
| Implement an issue up to the push | issue-implementer |
| Docblocks, README, changelog | doc-writer |
| Triage and label issues | issue-triager |
| PR description | pr-writer |
| Release notes | release-notes |

Standard workflow: planner → task-splitter → issue-writer → issue-implementer → code review → pr-writer.

The code review is done by the review agent of the stack plugin, if installed (`php-agents:php-code-reviewer`, in Symfony projects `symfony-agents:symfony-code-reviewer`, in React frontends `react-agents:react-code-reviewer`).

## Context when delegating

Subagents do not see the chat history. Every call contains:

1. Goal and success criterion in 1-2 sentences
2. Paths, issue numbers, branch names (no full file contents)
3. Decisions already made and constraints
4. Expected report format

Store long plans as a file (`PLAN.md`) or as an issue and pass only the reference.
