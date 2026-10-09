---
name: delegation-routing
description: Use when delegating planning, debugging, refactoring, documentation, GitHub issue, pull request or release work to subagents, or when choosing which agent fits a task. Contains the routing table, the standard workflow and rules for handing over context.
---

# Delegation (Workflow)

Delegiere nach Entscheidungskomplexität, nicht nach Aufgabenart. Das Modell steht im Frontmatter des jeweiligen Agents. Gemeinsame Regeln: Skill `developer-workflow-agents:engineering-conventions`.

| Aufgabe | Agent |
|---|---|
| Unklares Design, Architektur, Trade-offs | planner |
| Bug ohne erkennbare Ursache | debugger |
| Umbau ohne Verhaltensänderung | refactorer |
| Pläne in Aufgaben zerlegen | task-splitter |
| Issues anlegen | issue-writer |
| Issue bis PR umsetzen | issue-implementer |
| Docblocks, README, Changelog | doc-writer |
| Issues sichten, labeln | issue-triager |
| PR-Beschreibung | pr-writer |
| Release Notes | release-notes |

Standardablauf: planner → task-splitter → issue-writer → issue-implementer → Code-Review → pr-writer.

Das Code-Review übernimmt der Review-Agent des Stack-Plugins, falls installiert (`php-agents:php-code-reviewer`, in Symfony-Projekten `symfony-agents:symfony-code-reviewer`).

## Kontext beim Delegieren

Subagents sehen den Chatverlauf nicht. Jeder Aufruf enthält:

1. Ziel und Erfolgskriterium in 1-2 Sätzen
2. Pfade, Issue-Nummern, Branch-Namen (keine kompletten Dateiinhalte)
3. bereits getroffene Entscheidungen und Einschränkungen
4. gewünschtes Rückgabeformat

Lange Pläne als Datei (`PLAN.md`) oder Issue ablegen und nur den Verweis übergeben.
