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

**Variante A — CLI (aus dem Repo-Wurzelordner):**

```bash
npm i -g vercel      # falls noch nicht installiert
vercel               # Preview-Deploy (fragt einmalig nach Login/Projekt)
vercel --prod        # Produktions-Deploy
```

**Variante B — Git + Vercel-Dashboard:**

1. Auf vercel.com „New Project" → dieses Repo (`surroforge-website`) importieren.
2. **Root Directory** auf dem Standard (Repo-Wurzel) lassen, Framework Preset = „Other".
3. Deploy — kein Build-Schritt nötig.

## Als A4-PDF drucken

Die Seite hat ein eigenes Druck-Layout: im Browser `Strg/Cmd + P` → Ziel „Als PDF sichern",
Format **A4 hoch**. Statt der dunklen Website erscheint dann eine **helle, einseitige
Datenblatt-Fassung** (`@media print` / `@page A4` in `index.html`) mit Kennzahlen-Band,
Funktionsübersicht, den drei CFD-/Surrogat-Diagrammen und Autorenzeile. Ränder auf „Standard"
lassen; „Hintergrundgrafiken" sind optional (nur die dezenten Kennzahlen-Flächen hängen daran).

## Enthaltenes Beispiel

Der Abschnitt „Beispiel" zeigt das NACA-6412-Beispiel aus der Präsentation:
26 Anstellwinkel × 5 Anströmgeschwindigkeiten = 130 CFD-Rechnungen (OpenFOAM), daraus ein
ML-Surrogat (POD + MLP). Diagramme: Geschwindigkeitsfeld-Vergleich (Testfälle α = 8°/70 m/s
und α = 22°/30 m/s) sowie die Beiwert-Kurven c_L und c_D über dem Anstellwinkel.

## Noch anzupassen

- Platzhalter-Links (`href="#"`) auf echte Ziele setzen: Doku, Kontakt/Schnellstart.
- Optional: eigenes `favicon` und ein OG-Vorschaubild (`assets/…`) in den `<meta property="og:*">` ergänzen.
- Die Bilder in `assets/` stammen aus der Präsentation; bei Aktualisierung dort einfach neu kopieren.
