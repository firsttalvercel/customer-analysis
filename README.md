# Customer Usage Analyzer — MuleSoft

A single-page internal sales tool that parses MuleSoft customer reports and turns them into a professional, interactive analytics dashboard — all in the browser, with no data ever leaving the device.

---

## What it does

Drop any supported MuleSoft report file onto the page and instantly get a tailored dashboard with two analysis modes:

### 📊 Overview Mode
Best for **Running Applications CSVs** — shows a platform-level snapshot:
- Total messages, applications, vCores, workers, and flow counts
- Volume by application (bar chart + donut distribution)
- Per-app details: environment, workers, vCores, avg daily flows, avg monthly messages, total messages, share
- Platform insights: top app, traffic concentration, infrastructure footprint, complexity

### 📈 Month-over-Month Breakdown Mode
Best for **Message Usage PDFs and time-series CSVs** — shows usage trends over time:
- Daily Consumption chart (every individual day, Anypoint-style)
- Monthly Message Volume chart with rolling trend line
- Month-over-Month Change chart (green/red bars)
- Top Peak Days chart and ranked table
- Monthly breakdown table with MoM badges
- Trend insights: CMGR, 3-month trajectory, projection, usage consistency, anomaly detection

### Shared across both modes
- Executive summary with health score ring (0–100)
- KPI summary cards
- Auto-generated insights
- One-click PDF download via browser print (`@media print` optimised for A4)
- Privacy-first: all data is wiped from memory on reset, new file load, or page close

---

## Quick start

### Option 1 — Open directly (no server needed)
```bash
open index.html
```

### Option 2 — Local dev server
```bash
npm run dev
# → http://localhost:3000
```

### Option 3 — Deploy to Vercel
```bash
vercel --prod
```

---

## Supported file formats

| Format | Notes |
|--------|-------|
| `.csv` / `.tsv` | Auto-detects tab vs comma delimiter; handles UTF-16 LE/BE BOM (Anypoint exports) |
| `.xlsx` / `.xls` | Full Excel support via SheetJS |
| `.pdf` | Client-side extraction via PDF.js — Anypoint Message Usage Report format |
| `.html` | Raw HTML export from the Anypoint usage report generator |
| `.json` | Array of records or `{ monthly: [...] }` object |

**Missing column headers** are handled automatically — if the second column has no header (common in Anypoint CSV exports) the parser finds the first numeric column positionally.

---

## Supported report types

### Running Applications Report (CSV)
Exported from Anypoint Platform. Expected columns (detected automatically, order doesn't matter):

| Column | Description |
|--------|-------------|
| `APPLICATION_NAME` | App identifier |
| `ENVIRONMENT_TYPE` | `production` or `sandbox` |
| `TOTAL_ACTIVE_VCORES` | vCores allocated |
| `WORKER_COUNT` | Worker instances |
| `AVG_DAILY_FLOW_COUNT` | Average flows triggered per day |
| `AVG_MONTHLY_MESSAGES` | Average monthly message volume |
| `TOTAL_MESSAGES_OVER_MONTHS_OF_DATA` | Total messages (used for ranking) |
| `MONTHS_OF_DATA` | Data window in months |

### Message Usage Report (PDF or HTML)
Standard Anypoint platform export. The parser extracts:
- KPI header (Total Messages, Active Days, Peak Day, Avg/Active Day)
- Monthly breakdown table (month, messages, share, MoM change)
- Top 10 Peak Days table

### Daily / Monthly CSV
Any two-column file with dates and message counts. Dates in any common format (`M/D/YYYY`, `YYYY-MM-DD`, etc.).

---

## Privacy & data handling

All file processing happens **entirely in the browser**. No customer data is transmitted to any server at any point.

In addition, the app actively clears data from memory:

| Trigger | What is cleared |
|---------|-----------------|
| "+ New Report" button | All chart instances, DOM tables, KPI cards, file input blob |
| Loading a new file | Previous report data wiped before new file is parsed |
| Tab close / navigate away | `pagehide` + `beforeunload` listeners clear all in-memory data |

---

## Tech stack

| Library | Version | Purpose |
|---------|---------|---------|
| [PDF.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | Client-side PDF text extraction |
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Bar, line, and doughnut charts |
| [SheetJS / xlsx](https://sheetjs.com/) | 0.18.5 | CSV, TSV, Excel parsing |

Everything else is vanilla HTML, CSS, and JavaScript — no build step, no framework, no dependencies to install.

---

## Project structure

```
index.html        — entire application (HTML + CSS + JS, single file)
package.json      — project metadata and dev scripts
.gitignore        — excludes node_modules, .vercel, .env, secrets
README.md         — this file
```

---

## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start local server on port 3000 (`npx serve .`) |
| `npm run build` | No-op (no transpilation needed) |
| `npm run deploy` | Deploy to Vercel production |
