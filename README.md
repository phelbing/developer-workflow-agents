# developer-workflow-agents

10 Subagents mit Modell-Routing (haiku, sonnet, opus) für den Arbeitsablauf eines Entwicklers: Planung, Debugging, Refactoring, Dokumentation, GitHub Issues, Pull Requests und Release Notes. Dazu die gemeinsamen Entwicklungs-Konventionen. Sprach- und stack-unabhängig und die Basis für `php-agents` und `symfony-agents` (eigene Repos).

## Installation

```bash
claude plugin marketplace add <owner>/developer-workflow-agents
claude plugin install developer-workflow-agents@developer-workflow-agents
```

Danach in Claude Code `/agents` ausführen. Die Agents heißen mit Plugin-Präfix, z. B. `developer-workflow-agents:planner`.

## Enthalten

| Modell | Agents |
|---|---|
| opus | planner, debugger |
| sonnet | refactorer, task-splitter, issue-writer, issue-implementer |
| haiku | doc-writer, issue-triager, pr-writer, release-notes |

Skills:
- `engineering-conventions`: gemeinsame Regeln für Vorgehen, Qualität, Review, Git, Sicherheit und Rückgabeformat.
- `delegation-routing`: Routing-Tabelle, Standardablauf (planner → task-splitter → issue-writer → issue-implementer → Code-Review → pr-writer) und Regeln für die Kontext-Übergabe.

## Aufbau der Konventionen

Jede Regel steht genau einmal, auf der allgemeinsten Ebene, für die sie gilt. Höhere Ebenen laden die tieferen und ergänzen nur.

| Ebene | Plugin | Skill |
|---|---|---|
| Basis (alle Stacks) | developer-workflow-agents | `engineering-conventions` |
| PHP | php-agents | `php-conventions`, `php-security-checklist`, `php-design-patterns` |
| Symfony | symfony-agents | `symfony-conventions`, `symfony-security-checklist` |

Die Agents laden ihren Skill über das Skill-Tool, der Skill lädt die Ebene darunter. Lässt sich ein Skill nicht laden, melden die Agents das in der Rückgabe.

## Empfohlene Ergänzungen im eigenen Projekt

Ein Plugin lädt keine `CLAUDE.md` und keine Berechtigungen ins Projekt. Beides liegt als Vorlage unter `examples/`:

- `examples/CLAUDE.template.md`: Block für die eigene `CLAUDE.md`. Verweist nur auf die Skills.
- `examples/settings.json`: Berechtigungen für `.claude/settings.json`. Erlaubt `git` und `gh` im nötigen Umfang, sperrt Force-Push, Push auf `main`/`master`, `ssh`/`scp`/`rsync` und das Lesen von `.env*`.

## Hinweise

- `CLAUDE_CODE_SUBAGENT_MODEL` nicht setzen, sonst überschreibt die Variable die `model:`-Zeilen aller Agents.
- Modell eines Agents ändern: Zeile `model:` im Frontmatter unter `agents/`.
- Fremde Agents vor dem Einsatz lesen und Tool-Rechte prüfen.
- `gh` muss angemeldet sein (`gh auth status`).

## Für Maintainer

```bash
claude plugin validate ./
```

`version` in `.claude-plugin/plugin.json` ist gesetzt. Nutzer bleiben auf dieser Version, bis du sie erhöhst. Abhängigkeiten ohne Versionsbereich folgen dem jeweils aktuellen Stand der anderen Plugins. Für feste Versionsbereiche die Releases mit `claude plugin tag --push` taggen und die Bereiche in `dependencies` eintragen.

## Lizenz

MIT, siehe `LICENSE`.
