---
name: engineering-conventions
description: Use at the start of any coding, review, refactoring, debugging or git task, and whenever another skill or agent refers to the shared engineering conventions. Stack-independent rules for scope, testing, git, secrets, review and reporting.
---

# Gemeinsame Entwicklungs-Konventionen

Basis für alle Agents und Stacks. Stack-spezifische Skills (z. B. `php-agents:php-conventions`, `symfony-agents:symfony-conventions`) ergänzen diese Regeln und wiederholen sie nicht.

## Vorgehen

1. Erst das Problem benennen, dann die Lösung wählen. Die kleinste Lösung, die es löst.
2. Betroffene Dateien und Nachbarcode lesen. Konventionen des Projekts gehen vor.
3. Kleiner Diff, keine Änderungen außerhalb des Auftrags. Entdeckte Nebenbaustellen melden, nicht beheben.
4. Unklar oder widersprüchlich: nachfragen, nicht raten. Annahmen offenlegen.

## Qualität

- Vor dem Abschluss Tests, Static Analysis und Code-Style laufen lassen. Ergebnis ehrlich berichten.
- Neues Verhalten braucht einen Test. Ein Test, der nicht rot werden kann, ist wertlos.
- Review vor jedem Commit bzw. PR. Migrationen sowie Änderungen an Zahlung, Login und Berechtigungen zusätzlich fachlich prüfen lassen.

## Review

Prüfreihenfolge: Korrektheit, Sicherheit, Datenzugriff (N+1, Transaktionen, Migrationen), Fehlerbehandlung, Tests, Entwurf (unnötige Abstraktionen, versteckte Abhängigkeiten), Stil.

Rückgabe, nach Schwere sortiert:
- **Blocker** – `datei:zeile` – Problem – Vorschlag
- **Sollte** – ...
- **Nit** – ...

Nur echte Befunde, kein Lob, keine Wiederholung des Diffs. Keine Befunde: "Keine Beanstandungen" und ein Satz, was geprüft wurde.

## Git

- Nie auf `main`/`master` pushen, kein Force-Push, kein `reset --hard`.
- Ein Branch pro Aufgabe (`feature/<nr>-<slug>`, `fix/<nr>-<slug>`), kleine Commits mit sprechender Nachricht.
- Issues, Branches und Repositories nicht löschen.

## Sicherheit und Umgebung

- Secrets (`.env*`, Schlüssel, Tokens) nicht lesen, nicht ausgeben, nicht committen. Fundstellen nennen, nie Werte.
- Kein Zugriff auf Produktionsserver. Deployments macht der Mensch.
- Läuft die Entwicklung in Docker, Befehle im Container ausführen: `docker compose exec <service> ...`. Servicenamen in `docker-compose.yml` prüfen.
- Dateien in `vendor/` und `node_modules/` nicht ändern.

## Rückgabe

- Ergebnis zuerst, dann Belege. Fundstellen als `pfad:zeile`.
- Offene Punkte und Annahmen am Ende nennen.
