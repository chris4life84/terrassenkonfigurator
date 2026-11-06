# Terrassenkonfigurator – Projektanweisungen für Claude

Ein Terrassenkonfigurator, der in eine bestehende Website eingebettet wird.
Repo: https://github.com/chris4life84/terrassenkonfigurator

## Wichtig: Zwei-Rechner-Betrieb

Es wird abwechselnd an zwei Rechnern entwickelt. **Das Git-Repo ist die einzige Quelle der Wahrheit.**

- Projektwissen (Entscheidungen, Stand, nächste Schritte) gehört in `docs/*.md` – **nicht** in Claudes lokales Memory (das ist pro Rechner und wird nicht synchronisiert).
- Zu Beginn jeder Sitzung: `git pull` ausführen und `docs/STATUS.md` lesen.
- Am Ende jeder Sitzung: `docs/STATUS.md` aktualisieren, committen und pushen.
- Details: `docs/ARBEITSWEISE.md`

## Dokumentation

| Datei | Inhalt |
|---|---|
| `docs/STATUS.md` | Aktueller Stand, nächste Schritte, offene Fragen – **zuerst lesen** |
| `docs/KONZEPT.md` | Prozess- und Architekturkonzept (Entwurf) |
| `docs/ANFORDERUNGEN.md` | Was der Konfigurator können soll |
| `docs/ENTSCHEIDUNGEN.md` | Technische Entscheidungen mit Begründung (Log) |
| `docs/ARBEITSWEISE.md` | Workflow für zwei Rechner, Git-Regeln |

Neue Entscheidungen werden in `docs/ENTSCHEIDUNGEN.md` eingetragen, bevor oder während sie umgesetzt werden.

## Konventionen

- Sprache der Doku und Commits: Deutsch
- Zeilenenden: LF (über `.gitattributes` erzwungen)
- Keine Secrets ins Repo – lokale Werte in `.env` (gitignored), Vorlage in `.env.example`
