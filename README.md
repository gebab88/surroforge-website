# SurroForge — One-Pager

Statische Landing-Page (ein einziges `index.html`, keine Build-Tools). Alle Styles sind inline,
Schriften kommen über Google Fonts (mit System-Fallback), Bilder liegen unter `assets/`.

```
landing/
├── index.html      # die komplette Seite
├── assets/         # die referenzierten CFD-/Koeffizienten-Bilder
├── vercel.json     # Cache-/Sicherheits-Header (optional)
└── README.md
```

## Auf Vercel deployen

Kein Framework, kein Build-Schritt — Vercel serviert den Ordner direkt.

**Variante A — CLI (aus diesem Ordner):**

```bash
npm i -g vercel      # falls noch nicht installiert
cd landing
vercel               # Preview-Deploy (fragt einmalig nach Login/Projekt)
vercel --prod        # Produktions-Deploy
```

**Variante B — Git + Vercel-Dashboard:**

1. Ordner in ein Git-Repo legen und zu GitHub/GitLab pushen.
2. Auf vercel.com „New Project" → Repo importieren.
3. **Root Directory** auf `landing` setzen, Framework Preset = „Other". Deploy.

## Als A4-PDF drucken

Die Seite hat ein eigenes Druck-Layout: im Browser `Strg/Cmd + P` → Ziel „Als PDF sichern",
Format **A4 hoch**. Statt der dunklen Website erscheint dann eine **helle, einseitige
Datenblatt-Fassung** (`@media print` / `@page A4` in `index.html`). Tipp: „Hintergrundgrafiken"
im Druckdialog aktivieren, damit die Akzentfarben mitkommen. Ränder auf „Standard" lassen.

## Enthaltenes Beispiel

Der Abschnitt „Beispiel" enthält den **Versuchsraum von Folie 5** der Präsentation:
26 Anstellwinkel × 5 Anströmgeschwindigkeiten = 130 CFD-Rechnungen am NACA-6412-Profil,
inklusive DoE-Gitter mit den Testfällen α = 8°/70 m/s und α = 22°/30 m/s.

## Noch anzupassen

- Platzhalter-Links (`href="#"`) auf echte Ziele setzen: Doku, Kontakt/Schnellstart.
- Optional: eigenes `favicon` und ein OG-Vorschaubild (`assets/…`) in den `<meta property="og:*">` ergänzen.
- Die Bilder in `assets/` stammen aus der Präsentation; bei Aktualisierung dort einfach neu kopieren.
