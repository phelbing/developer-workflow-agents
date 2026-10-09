# Architecture

## 1. Structure of the conventions

Every rule exists exactly once, on the most general level it applies to. Each level loads the level it builds on and only adds to it.

| Level | Plugin | Builds on | Skills |
|---|---|---|---|
| Base (all stacks) | developer-workflow-agents | – | `engineering-conventions`, `delegation-routing` |
| PHP | php-agents | developer-workflow-agents | `php-conventions`, `php-security-checklist`, `php-design-patterns`, `php-delegation-routing` |
| Symfony | symfony-agents | php-agents | `symfony-conventions`, `symfony-security-checklist`, `symfony-delegation-routing` |
| React | react-agents | developer-workflow-agents | `react-conventions`, `frontend-security-checklist`, `react-delegation-routing` |

The agents load their skills at startup through the `skills` field in the frontmatter, including every level they build on. In the main conversation, a conventions or checklist skill loads the level it builds on through the Skill tool. The delegation routing skills stand alone and do not load each other. If a skill cannot be loaded, the agents say so in their report.
