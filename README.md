# Bold – Metronic 9.2.5 Tailwind

Denne repository indeholder en tilpasset opsætning af Metronic 9 (Tailwind-udgaven) for HTML-projekter. Koden er baseret på Metronic 9.2.5 og anvender Tailwind CSS v4, PostCSS og Webpack til at bygge aktiver (CSS/JS) samt bundte tredjeparts‑vendors (ApexCharts, Leaflet, KeenIcons m.fl.).

Bemærk: Metronic og tilhørende assets kræver gyldig licens fra KeenThemes. Se licensafsnittet nederst.

## Indhold
- Overblik og formål
- Krav og installation
- Scripts (udvikling og build)
- Projektstruktur
- Styling og theming (Tailwind, KTUI, farver, dark mode, RTL)
- Vendors og ikoner
- HTML Composer (hurtig generering)
- Deployment
- Fejlfinding og tips
- Links til dokumentation og licens

## Overblik og formål
- Metronic 9.2.5 Tailwind template: En moderne, komponent‑drevet admin/portal front‑end med Tailwind 4 og KTUI.
- Brugsscenarie: Hurtig opstart af dashboards, adminpaneler og applikations‑UI med standardiserede komponenter, layouts og værktøjer.

## Krav og installation
- Node.js: Anbefalet LTS (>= 18). Kontroller lokal version med `node -v` og `npm -v`.
- Installation:
  1. Installer afhængigheder: `npm install`
  2. Byg CSS: `npm run build:css`
  3. Byg JS: `npm run build:js`

Afhængigheder og værktøjer:
- Tailwind CSS v4 (via `@tailwindcss/cli`)
- PostCSS med `postcss-import`, `postcss-preset-env`, nesting
- Webpack 5 (transpilering af TS/JS, bundling, minificering)
- KTUI komponenter (`@keenthemes/ktui`)

## Scripts
Defineret i package.json:
- `npm run build:css` – Bygger Tailwind CSS fra `src/css/styles.css` til `public/assets/css/styles.css`.
- `npm run build:css:watch` – Watch‑mode for Tailwind CSS (genbygger ved ændringer).
- `npm run build:js` – Bygger JS/TS via Webpack i udviklingstilstand.
- `npm run build:prod` – Producerer minificeret CSS og JS til produktion.
- `npm run lint` – ESLint for TypeScript/JS i `src/core`.

Anbefalet udviklingsworkflow:
- Kør i to terminaler:
  - Terminal A: `npm run build:css:watch`
  - Terminal B: `npx webpack --env development` (eller `npm run build:js` hvis du kun ønsker ét build)

Output placeres i `public/assets` (CSS/JS, vendors, media).

## Projektstruktur (uddrag)
- `src/css/styles.css` – Hoved‑CSS, importerer Tailwind, KTUI, komponenter og demo‑tema.
  - Anvender Tailwind v4 @source til scan af utility‑classes: se linje med `@source '../../dist/assets/vendors/ktui/'`.
  - Importerer KTUI styles: `../../node_modules/@keenthemes/ktui/styles.css`.
- `src/core/` – KT komponent bootstrap (fx menu, helpers). Entrypoint: `src/core/index.ts` (eksporterer `KTComponents`, `KTMenu`, `KTUtils` m.fl.).
- `src/app/` – Applikationsspecifik JS (fx layouts). Eksempel: `src/app/layouts/demo1.js` håndterer sidebar/mega menu.
- `src/vendors/` – Lokale vendor‑assets til kopiering/bundling (jf. `webpack.vendors.js`).
- `public/` – Byggede aktiver og statiske filer (åbnes direkte i browser ved statisk hosting).
- `webpack.config.js` – Webpack opsætning med auto‑watch i development, TS/JS rules, PostCSS, Copy & Merge vendors.
- `webpack.vendors.js` – Definerer hvilke vendor‑filer der kopieres/bundtes (KeenIcons, ApexCharts, Leaflet, PrismJS, Clipboard, KTUI m.m.).
- `postcss.config.js` – PostCSS plugins (import, preset env, nesting, Tailwind, autoprefixer).
- `tsconfig.json` – TypeScript compiler‑konfiguration (ES5 target for bred browser‑support).

## Styling og theming
- Tailwind 4: Utility‑first styling. Konfiguration foregår primært i CSS via imports og @source. Du kan tilføje globale tema‑variabler og tokens i `src/css/config.ktui.css` og komponentspecifik styling under `src/css/components/`.
- KTUI komponenter: Metronic Tailwind anvender KTUI til interaktive komponenter (dropdowns, menu, toggle osv.). `window.KTComponents.init()` kaldes automatisk ved DOM ready.
- Farver og design tokens: Justér i `src/css/config.ktui.css` og evt. i demo‑CSS (`src/css/demos/demo1.css`).
- Dark mode: Metronic Tailwind understøtter dark mode. Typisk styres via en `dark` class på `<html>`/`<body>` og tilsvarende Tailwind/KTUI varianter. Se dokumentationen for anbefalet toggling og persistens.
- RTL: Højre‑til‑venstre sprog understøttes via Metronic build‑pipeline og Tailwind utilities. Følg dokumentationen for at aktivere RTL builds og class‑strategier.

Relevante dokumentationsafsnit:
- Getting Started (opsætning, struktur)
- Styles & Theming (Tailwind 4, tokens, farver)
- Components & Plugins (KTUI, interaktioner)
- Layout (header, sidebar, content, containers)
- Dark Mode og RTL

Se links i afsnittet “Dokumentation”.

## Vendors og ikoner
Vendor‑assets styres af `webpack.vendors.js`:
- KeenIcons: CSS og fontfiler pakkes til `dist/assets/vendors/keenicons/*` og kopieres til `public/assets/vendors/keenicons` via Webpack.
- ApexCharts: CSS/JS til simple charts.
- Leaflet (+ Esri): Kortkomponenter med styles, JS og billeder.
- PrismJS: Kodehighlighting (med init script i `src/vendors/prismjs/prismjs.init.js`).
- Clipboard: Kopi til udklipsholder.
- KTUI: `@keenthemes/ktui/dist/ktui.min.js` gøres tilgængelig under vendors.

Hvis du tilføjer nye vendors, udvid `webpack.vendors.js` med relevante `src` og `dist` mål samt `bundle: true` når de skal merges til én fil.

## HTML Composer
Denne repo indeholder `src/html-composer.html`, som viderestiller til Metronic Tailwind HTML Composer. Brug den til at generere sider/layouts hurtigt og kopier herefter markup ind i dine skabeloner/sider i `public/` eller i din app‑ramme.

## Deployment
- Produktion: `npm run build:prod` genererer minificeret CSS og JS.
- Statisk hosting: Deploy indholdet af `public/` til din hosting/CDN. Sørg for at relative stier til assets bevares (`/assets/...`).
- Cache busting: Filnavne er statiske; overvej HTTP cache headers/versionering ved større udrulninger.

## Fejlfinding og tips
- Ingen dev‑server medfølger. Brug `build:css:watch` og Webpack i watch tilstand under udvikling. Åbn HTML‑filer direkte fra `public/` (eller servér mappen via en simpel statisk server).
- ESLint: Kør `npm run lint` for at fange typiske problemer i `src/core`.
- Tailwind scanning: Husk at pege @source mod alle steder hvor du bruger utilities (komponentbiblioteker, dine skabeloner osv.).
- Webpack env: Produktion bygges med `--env production` (allerede i `build:prod`). Til udvikling kan du køre `npx webpack --env development` for sikker watch‑adfærd.

## Dokumentation
- Metronic Tailwind Docs (side menu dækker alle sektioner):
  - https://keenthemes.com/metronic/tailwind/docs/index.html
- Udvalgte sektioner:
  - Getting Started / Installation
  - Build & Tools (Tailwind 4, PostCSS, Webpack)
  - KTUI Components
  - Layout & Utilities
  - Dark Mode
  - RTL
  - Icons (KeenIcons)
  - Plugins (ApexCharts, Leaflet, PrismJS, Clipboard)
  - HTML Composer

## Licens
- Metronic er et kommercielt produkt fra KeenThemes og kræver gyldig licens. Se: https://keenthemes.com/
- Denne repository indeholder konfiguration og integrationer målrettet Metronic 9.2.5 Tailwind. Sørg for at din brug overholder licensvilkår.
"# Metronic-9.2.5" 
