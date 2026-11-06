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
