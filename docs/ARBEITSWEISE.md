# Arbeitsweise mit zwei Rechnern

Ziel: Egal an welchem Rechner gearbeitet wird – der Stand ist identisch.

## Grundregel

**Alles, was zählt, liegt im Git-Repo.** Was nicht gepusht ist, existiert für den anderen Rechner nicht.

## Sitzungs-Ablauf

**Start:**
```bash
git pull
```
Dann `docs/STATUS.md` lesen (Claude macht das automatisch über `CLAUDE.md`).

**Ende:**
1. `docs/STATUS.md` aktualisieren (Stand, nächste Schritte, Verlaufseintrag)
2. Committen und pushen:
```bash
git add -A
git commit -m "Kurze Beschreibung"
git push
```

Nie mit ungepushten Änderungen den Rechner wechseln. Im Zweifel einen WIP-Commit pushen.

## Was wird synchronisiert – und was nicht

| Synchronisiert (im Repo) | Nicht synchronisiert (lokal) |
|---|---|
| Code, `docs/*.md`, `CLAUDE.md` | Claudes Memory (`~/.claude/projects/...`) |
| `.claude/settings.json` (Projekt-Einstellungen) | `.claude/settings.local.json` |
| `package.json` + `package-lock.json` | `node_modules/` → per `npm ci` neu installieren |
| `.env.example` | `.env` (Secrets) |

Deshalb: Wissen und Entscheidungen immer in `docs/` festhalten, nicht nur im Chat.

## Gleiche Umgebung auf beiden Rechnern

- **Zeilenenden:** `.gitattributes` erzwingt LF – keine Diffs durch CRLF/LF.
- **Editor:** `.editorconfig` legt Einrückung/Encoding fest.
- **Node-Version:** wird in `.nvmrc` und `package.json` (`engines`) fixiert, sobald der Stack steht.
- **Abhängigkeiten:** immer `package-lock.json` committen und mit `npm ci` installieren (exakt gleiche Versionen).

## Einrichtung des zweiten Rechners (einmalig)

```bash
git clone https://github.com/chris4life84/terrassenkonfigurator.git
```
Danach (sobald Code existiert) `npm ci`. Git-Login läuft über den Git Credential Manager (Browser-Login beim ersten Push).
