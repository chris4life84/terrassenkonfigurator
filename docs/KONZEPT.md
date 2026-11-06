# Konzept (Entwurf)

> Stand 2026-11-06 – Diskussionsgrundlage, noch nicht entschieden. Entscheidungen wandern nach `ENTSCHEIDUNGEN.md`.

## Ausgangslage

- Website **modul-garten.de**: WordPress 7.1 + WooCommerce 10.3 + Divi, Apache-Hosting.
- Kleines Team, begrenzte Kapazitäten. Planbare Vorbestellungen sind gewünscht.
- Start mit dem Terrassenkonfigurator. Später sollen Pergola- und Modulhauskonfigurator folgen.

## Empfohlener Geschäftsprozess

Kernidee: **Der Konfigurator definiert das Sortiment.** Was man nicht konfigurieren kann, wird nicht angeboten oder nur als bezahlter Sonderwunsch. Das verhindert, dass man beim Kunden „vom Hundertstel ins Tausendstel" kommt.

| Stufe | Was passiert | Status der Konfiguration |
|---|---|---|
| 1. Konfigurieren | Kunde konfiguriert online und sieht einen **Richtpreis** („vorbehaltlich Aufmaß"). Die Konfiguration wird mit ID/Link gespeichert. | Entwurf |
| 2. Vorbestellen | Kunde bucht eine **Aufmaß- und Planungspauschale** (z. B. 149–299 €). Wird der Auftrag erteilt, wird sie verrechnet. Andernfalls bleibt sie als Aufwandsentschädigung. Die Zahlung läuft über WooCommerce. | Vorbestellt |
| 3. Aufmaß | Das Team prüft die Konfiguration vor Ort, und zwar **mit demselben Tool** im Team-Modus. Es gibt eine feste Checkliste und ein festes Zeitfenster. Änderungen sind nur innerhalb der Baukasten-Optionen möglich. | Aufmaß erfolgt |
| 4. Bestätigen | Die angepasste Konfiguration (Version 2) mit **Festpreis** geht an den Kunden. Der Kunde bestätigt online und leistet eine Anzahlung. | Bestätigt |
| 5. Umsetzen | Produktion und Montage im geplanten Kalenderwochen-Slot. | In Umsetzung |

Regeln gegen Ausufern:
- Im Preis ist **eine Planungsrevision** enthalten. Jede weitere ist kostenpflichtig.
- Sonderwünsche außerhalb des Baukastens gibt es nur als separates, bezahltes Angebot.
- Option für einfache Fälle: **Selbst-Aufmaß** durch den Kunden (geführte Fotos und Maße, Videocall). Vor Ort nur, wenn nötig, z. B. bei Hanglage oder unklarem Untergrund.
- Vorbestellungen in **Kontingenten pro Kalenderwoche**. Das macht die Kapazität planbar.

Rechtliches (Widerruf bei Maßanfertigung, AGB zur Pauschale) muss geprüft werden. Das ist hier nicht abschließend geklärt.

## Empfohlene Architektur

**Kein Shopify, kein eigener Shop.** Gründe:
- WooCommerce ist bereits vorhanden und übernimmt Zahlung, Rechnungen und Kundenkonten.
- Shopify kann komplexe konfigurierbare Produkte nicht nativ abbilden. Man bräuchte dort ohnehin eine eigene App.

Der Konfigurator wird eine **eigene, von WordPress entkoppelte Anwendung**. So überlebt er auch einen späteren Shop- oder Theme-Wechsel.

```
modul-garten.de (WordPress/WooCommerce)
   └─ Mini-WP-Plugin: Shortcode lädt <terrassen-konfigurator>
          │
          ▼
   Konfigurator-Frontend (Web Component, React)
          │  speichert Konfiguration, holt Preise
          ▼
   Konfigurator-API (Node/TypeScript) ── Datenbank (Konfigurationen, Versionen, Status)
          │  legt Bestellung/Pauschale an
          ▼
   WooCommerce REST-API (Zahlung, Rechnung)
```

### Bausteine (Monorepo)

| Paket | Inhalt | Wiederverwendung |
|---|---|---|
| `packages/engine` | Produktmodell, Regeln, Validierung, Preis, Stückliste. Reines TypeScript, ohne UI, mit Tests. | Wird von allen Konfiguratoren geteilt. Läuft im Browser **und** auf dem Server (Preisprüfung). |
| `packages/ui` | Gemeinsame Bausteine: Schritte, Preisbox, Zusammenfassung | Für alle Konfiguratoren |
| `apps/terrasse` | Terrassenkonfigurator (später `apps/pergola`, `apps/modulhaus`) | – |
| `apps/api` | Konfigurationen speichern und versionieren, Status, WooCommerce-Anbindung, E-Mails | Eine API für alle Konfiguratoren |
| `wp-plugin` | Shortcode `[mg_konfigurator typ="terrasse"]` | – |

### Stack-Vorschlag

- **TypeScript** überall. So gibt es eine Sprache für Frontend, Backend und die Rechenlogik.
- **React + Vite** für das Frontend. Es hat das größte Ökosystem und bietet mit react-three-fiber einen guten Weg zu 3D.
- **Einbettung als Web Component mit Shadow DOM**. Das isoliert den Konfigurator vom Divi-CSS. Ein iframe wäre die Alternative.
- **2D-Ansicht in SVG** zum Start, **3D (three.js)** als spätere Ausbaustufe.
- **API:** Node + Hono, Datenbank Postgres (z. B. mit Drizzle). Die Hosting-Frage ist noch offen.
- **Produktdaten und Preise** zunächst als versionierte Dateien im Repo. Später kann ein Admin-Bereich hinzukommen.
- **pnpm** als Paketmanager für das Monorepo, **Vitest** für Tests.

### Backend: in WordPress oder daneben?

Durchgespielt am Beispiel: Familie Becker konfiguriert 20 m², klickt auf „Jetzt vorbestellen" und bezahlt die Pauschale. Zwei Wochen später öffnet der Techniker beim Aufmaß dieselbe Konfiguration auf dem Tablet und ändert sie auf 18,5 m².

| | **A: alles in WordPress (Plugin)** | **B: eigene kleine API neben WordPress** |
|---|---|---|
| Wo liegt Beckers Konfiguration? | WordPress-Datenbank (eigene Tabellen) | Eigene Datenbank (Postgres) |
| Wer rechnet den Preis? | Browser in TypeScript, Server müsste in PHP nachrechnen (doppelte Logik) | Browser und Server nutzen **denselben** TypeScript-Code |
| Aufmaß-Tablet, Versionen, Kundenstatus, Kapazitätsplanung | Muss als WP-Admin-Erweiterung gebaut werden (mühsam) | Normale App-Funktionen |
| Zahlung | WooCommerce direkt | API legt WooCommerce-Bestellung an, WooCommerce meldet „bezahlt" zurück (Webhook) |
| Risiko | WP-/Plugin-Updates können den Konfigurator beeinflussen | Ein System mehr zu betreiben (Updates, Backups) |
| Kosten | keine zusätzlichen | ca. 5–20 €/Monat |
| Bei Shop-Wechsel | Neubau nötig | Konfigurator bleibt, nur die Shop-Anbindung wird getauscht |

**Empfehlung: B.** Der Konfigurator wird mit Aufmaß-Modus, Versionen und Terminplanung eine richtige Anwendung. WordPress bleibt Schaufenster und Kasse. Den Betrieb (Updates, Backups) übernimmt Claude mit.

Zum WordPress-Relaunch gehört ein eigenes Plugin `mg-konfigurator`. Es bindet den Konfigurator als Block bzw. Shortcode ein, stellt das Produkt „Vorbestellpauschale" bereit und meldet Zahlungen an die API.

## Ausbaustufen

1. **MVP:** Terrassenkonfigurator mit 2D-Ansicht und Richtpreis. Konfiguration speichern, Link per E-Mail versenden, Aufmaßpauschale über WooCommerce buchen. Das Team erhält eine Benachrichtigung.
   In dieser Stufe muss der Preis noch nicht serverseitig verbindlich geprüft werden. Bezahlt wird nur die feste Pauschale, der Festpreis kommt erst nach dem Aufmaß.
2. **Team-Modus:** Aufmaß im Tool, Versionen, Kunde bestätigt Version 2 online, Anzahlung.
3. **Komfort:** 3D-Ansicht, Kapazitätsplanung pro Kalenderwoche, Admin-Bereich für Preise.
4. **Weitere Konfiguratoren:** Pergola und Modulhaus auf derselben Engine.
