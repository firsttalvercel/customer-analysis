# Customer Usage Analyzer

A single-page sales tool that parses Vercel Message Usage Reports and turns them into visual dashboards — no backend, no login, no data leaves the browser.

## What it does

Drop a customer's **PDF or HTML** Message Usage Report onto the page and instantly get:

- **KPI summary** — Total messages, active days, peak day, and daily average
- **Monthly volume chart** — bar chart with the busiest month highlighted
- **Month-over-Month change chart** — green/red bars showing growth and decline
- **Auto-generated insights** — peak month, usage trend, consistency score, traffic concentration
- **Monthly breakdown table** — with color-coded MoM badges
- **Top peak days table** — ranked list of the highest-traffic days

## Usage

### Option 1 — Open locally

```bash
open index.html
```

No server needed. Works directly from the file system.

### Option 2 — Run with a local dev server

```bash
npx serve .
```

Then open [http://localhost:3000](http://localhost:3000).

### Option 3 — Deployed on Vercel

The site is linked to Vercel. To deploy:

```bash
vercel --prod
```

## Supported file formats

| Format | Notes |
|--------|-------|
| `.pdf` | Parsed in-browser via PDF.js — no upload to any server |
| `.html` | The raw HTML export from the usage report generator |

Both formats of the same report work. The parser uses two strategies (same-line and multi-line) and falls back automatically.

## Generated insights

| Insight | Description |
|---------|-------------|
| Peak Month | The single busiest calendar month |
| Usage Trend | Recent 3-month avg vs. prior 3-month avg |
| Usage Consistency | % of tracked days with activity |
| Traffic Concentration | What % of all traffic the top day drove |
| Top-3 Day Share | Combined share of the three busiest days |

## Tech stack

| Library | Version | Purpose |
|---------|---------|---------|
| [PDF.js](https://mozilla.github.io/pdf.js/) | 3.11 | Client-side PDF text extraction |
| [Chart.js](https://www.chartjs.org/) | 4.4 | Bar charts |

Everything else is vanilla HTML, CSS, and JavaScript — no build step, no dependencies to install.

## Project structure

```
index.html     — entire application (single file)
package.json   — project metadata
```

## Privacy

All file processing happens entirely in the browser. No customer data is sent to any server.
