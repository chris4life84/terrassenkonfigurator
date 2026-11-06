# Entscheidungen

> Jede wichtige technische Entscheidung wird hier mit Datum und Begründung festgehalten, damit sie auf beiden Rechnern (und später) nachvollziehbar ist.

## Vorlage

```
### E-XXX: Titel (JJJJ-MM-TT)
**Entscheidung:** …
**Begründung:** …
**Alternativen:** …
```

---

### E-001: Git-Repo als einzige Quelle der Wahrheit (2026-11-06)
**Entscheidung:** Projektwissen liegt als Markdown in `docs/`, Claude-Anweisungen in `CLAUDE.md`. Kein Verlass auf lokales Claude-Memory.
**Begründung:** Entwicklung an zwei Rechnern; lokales Memory wird nicht synchronisiert.
**Alternativen:** Memory-Ordner per Cloud-Sync teilen – fehleranfällig, nicht versioniert.

### E-002: Keine Projektanfragen außerhalb des Baukastens (2026-11-06)
**Entscheidung:** Was der Konfigurator nicht abbildet, wird nicht angeboten und freundlich abgesagt.
**Begründung:** Kleines Team (3–4 Personen), unnötigen Aufwand vermeiden.
**Alternativen:** Projektanfragen mit individuellem Angebot. Verworfen, weil sie die Kapazität binden.

### E-003: Vorbestellungen ganzjährig, Kapazität pro Woche einstellbar (2026-11-06)
**Entscheidung:** Kein Saisonmodell. Wochen lassen sich einzeln öffnen/sperren und mit einer Kapazität versehen (Start: 1 Terrasse/Woche).
**Begründung:** Flexibilität; Chris will selbst steuern können.
**Alternativen:** Feste Saison mit Kontingent.
