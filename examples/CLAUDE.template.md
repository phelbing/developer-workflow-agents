# Vorlage für die CLAUDE.md im Projekt

Ein Plugin kann keine `CLAUDE.md` mitliefern, die automatisch geladen wird. Diesen Block in die eigene `CLAUDE.md` übernehmen. Er verweist nur auf die Skills und wiederholt keine Regeln, damit sie nur an einer Stelle gepflegt werden.

---

## Entwicklung

- Gemeinsame Regeln (Vorgehen, Qualität, Review, Git, Sicherheit): Skill `developer-workflow-agents:engineering-conventions`.
- Delegation an Subagents und Standardablauf: Skill `developer-workflow-agents:delegation-routing`.
- Vor jedem Commit ein Code-Review durch den Review-Agent des Stack-Plugins.

## Projektspezifisch (hier eintragen)

- Docker-Servicename: `...`
- Testbefehl: `...`
- Static Analysis / Code-Style: `...`
