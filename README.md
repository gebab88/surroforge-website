# SurroForge — One-Pager

Statische Landing-Page (ein einziges `index.html`, keine Build-Tools). Alle Styles sind inline,
Schriften kommen über Google Fonts (mit System-Fallback), Bilder liegen unter `assets/`.

```
landing/
├── index.html       # die komplette Landing-Page
├── datenblatt.html  # eigenständige DIN-A4-Seite (self-contained, Fonts + Bilder eingebettet)
├── datenblatt.pdf   # daraus vorab gerendertes A4-PDF (das „fertige PDF")
├── assets/          # die referenzierten CFD-/Koeffizienten-Bilder
├── vercel.json      # Cache-/Sicherheits-Header + PDF-Header
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

## Das A4-Datenblatt (`datenblatt.html` + `datenblatt.pdf`)

Der komplette Inhalt der Landing-Page als **eigenständige DIN-A4-Seite** — nach dem Deploy erreichbar
unter `/datenblatt` (bzw. `datenblatt.html`) und direkt aus der Seite verlinkt (Navigation + Fußzeile).

- **`datenblatt.html`** ist **vollständig self-contained**: die Schriften (Sora, IBM Plex Sans/Mono)
  **und** alle Bilder sind als base64 eingebettet — **keine CDN-/Netzwerk-Abhängigkeit**, funktioniert
  offline und identisch auf jedem Host. Am Bildschirm ein A4-Blatt auf grauem Hintergrund mit
  Werkzeugleiste (*PDF herunterladen · Drucken/Als PDF speichern · Zur Website*); beim Drucken exakt
  **eine A4-Seite** (Werkzeugleiste wird ausgeblendet, `@page A4`).
- **`datenblatt.pdf`** ist das daraus **vorab gerenderte PDF** (Headless-Chromium, mit den echten
  Markenschriften) — das „fertige PDF" zum Verschicken. Verifiziert als **genau eine A4-Seite**.

Die dunkle Landing-Page (`index.html`) hat zusätzlich weiterhin ein eigenes `@media print`-Layout —
`Strg/Cmd + P` dort liefert dasselbe helle Datenblatt.

### `datenblatt.pdf` neu erzeugen

Wenn sich `datenblatt.html` ändert, das PDF mit Headless-Chromium neu rendern (Beispiel):

```bash
python -m playwright install chromium   # einmalig
python - <<'PY'
import asyncio; from playwright.async_api import async_playwright
async def main():
    async with async_playwright() as p:
        b = await p.chromium.launch(); pg = await b.new_page()
        await pg.goto("file://<ABSOLUTER-PFAD>/datenblatt.html", wait_until="load")
        await pg.evaluate("async () => { await document.fonts.ready; }")
        await pg.pdf(path="datenblatt.pdf", prefer_css_page_size=True, print_background=True)
        await b.close()
asyncio.run(main())
PY
```

## Enthaltenes Beispiel

Der Abschnitt „Beispiel" zeigt das NACA-6412-Beispiel aus der Präsentation:
26 Anstellwinkel × 5 Anströmgeschwindigkeiten = 130 CFD-Rechnungen (OpenFOAM), daraus ein
ML-Surrogat (POD + MLP). Diagramme: Geschwindigkeitsfeld-Vergleich (Testfälle α = 8°/70 m/s
und α = 22°/30 m/s) sowie die Beiwert-Kurven c_L und c_D über dem Anstellwinkel.

## Noch anzupassen

- Platzhalter-Links (`href="#"`) auf echte Ziele setzen: Doku, Kontakt/Schnellstart.
- Optional: eigenes `favicon` und ein OG-Vorschaubild (`assets/…`) in den `<meta property="og:*">` ergänzen.
- Die Bilder in `assets/` stammen aus der Präsentation; bei Aktualisierung dort einfach neu kopieren.
