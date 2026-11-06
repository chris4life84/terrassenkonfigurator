# Projektstatus

> Wird am Ende jeder Sitzung aktualisiert. Neueste Einträge oben im Verlauf.

## Aktueller Stand

Konzept-Entwurf in `KONZEPT.md` (Prozess + Architektur-Vorschlag). Noch kein Code, Tech-Stack noch nicht final entschieden – offene Fragen an Chris stehen unten.

## Nächste Schritte

1. Offene Fragen beantworten (unten) und `KONZEPT.md` gemeinsam finalisieren
2. Tech-Stack festlegen und in `ENTSCHEIDUNGEN.md` dokumentieren
3. Projekt-Setup (Build-Tool, Node-Version fixieren)

## Offene Fragen

- Wer entwickelt und wartet langfristig (nur Chris + Claude, oder auch Agentur/Entwickler)?
- Hosting: Wo läuft WordPress? Ist ein kleiner zusätzlicher Server/Managed-Dienst für die API okay (ca. 5–20 €/Monat)?
- Terrassen-Sortiment: festes Raster/Modulmaße oder frei wählbare Maße? Formen (Rechteck, L)? Montage inkl. oder auch Bausatz zum Selbstbau?
- Wer pflegt Preise – muss das Team ohne Entwickler Preise ändern können?
- Ist 3D zum Start verkaufsentscheidend oder reicht 2D?
- Zielgruppe/Region: Privatkunden? Einzugsgebiet fürs Aufmaß?
- Zeitplan: Soll das MVP zur Terrassensaison 2027 (ca. März) live sein?
- Weitere Punkte siehe `ANFORDERUNGEN.md`.

## Verlauf

### 2026-11-06 (2)
- Website analysiert: WordPress + WooCommerce + Divi
- `KONZEPT.md` angelegt: Stufenprozess mit Aufmaßpauschale, Architektur-Vorschlag (entkoppelte TS-App, Web Component, WooCommerce für Zahlung)

### 2026-11-06
- Repo initialisiert, Doku-Struktur angelegt (`CLAUDE.md`, `docs/`)
- `.gitattributes`, `.editorconfig`, `.gitignore` für rechnerübergreifend identisches Verhalten
