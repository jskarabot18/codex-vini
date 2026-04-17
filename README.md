# Codex Vini

*In vino veritas.*

An interactive personal wine atlas — every wine I have tasted, plotted on a map, filterable by country, region, grape, colour, and rating. A sibling to [The Grand Cru Atlas](https://jskarabot18.github.io/grand-cru-atlas/).

## Live site

**[→ Open Codex Vini](https://jskarabot18.github.io/codex-vini/)**

## What's inside

- **Interactive map** (Leaflet + CARTO basemap) with one pin per wine, sized by rating and coloured by grape variety
- **Full ledger** — sortable, searchable, filterable table of every wine tasted
- **Summary charts** — rating distribution and top countries, updated live as the collection grows
- **Cross-linked filters** — clicking any filter updates both the map and the table simultaneously
- **~120 wines at launch**, growing weekly

Each wine is tagged with:
- Producer, wine name, vintage, rating (0–10 scale), date tasted
- Country and region (pinned to a geographic centroid)
- Grape variety or named blend — drawn from a taxonomy of the 101 [TasteRank](https://tasterank.com/) varieties plus named wine categories (Bordeaux Blend, Barolo, Brunello di Montalcino, Châteauneuf-du-Pape Blend, etc.)
- Wine colour / style (red, white, rosé, sparkling)

## Files

| File | Purpose |
|---|---|
| `index.html` | The main app — self-contained HTML + CSS + JS |
| `wines.json` | Single source of truth — every wine as a JSON object |
| `grapes.json` | Grape taxonomy (101 varieties + named blends) |
| `regions.json` | Region → lat/lng lookup |
| `tools/add_wine.html` | Local form for adding new wines |
| `README.md` | This file |
| `LICENSE` | CC BY-NC 4.0 |

## How to use

Open the [live site](https://jskarabot18.github.io/codex-vini/) in any modern browser. Use the filters in the left panel to narrow by country, grape, colour, or rating. Click any pin on the map for a full wine card. Click any row in the ledger to fly the map to that wine. Press `Esc` or the ✕ Reset button to return to the full view.

To run locally, simply open `index.html` in a browser — no build tools or server required. *(If opening via `file://`, the browser may block loading `wines.json` via `fetch()`. Either use a local server — `python3 -m http.server` — or open `codex-vini-preview.html` which has the data embedded.)*

## Adding a new wine

### Option 1 — Local form (recommended)

1. Open `tools/add_wine.html` in a browser (via a local server, since it loads JSON)
2. Fill in the producer, wine, vintage, country/region, grape, and rating
3. Click **Generate JSON** and **Copy to clipboard**
4. Open `wines.json` and paste the generated block at the start of the array (immediately after the opening `[`)
5. Commit and push — GitHub Pages redeploys automatically

### Option 2 — Direct edit on GitHub

1. Open `wines.json` on github.com
2. Click the pencil icon to edit
3. Paste a new entry following the schema below
4. Commit

### Schema

Every wine is a JSON object of this shape:

```json
{
  "id": "clos-des-papes-cdp-2007",
  "producer": "Clos Des Papes",
  "wine": "Châteauneuf-du-Pape",
  "vintage": "2007",
  "country": "France",
  "region": "Southern Rhône",
  "grape": "Châteauneuf-du-Pape Blend",
  "color": "red",
  "rating": 10.0,
  "date_tasted": "2025-09-26",
  "lat": 44.056,
  "lng": 4.832
}
```

- `id` — stable slug of `producer-wine-vintage`, URL-safe
- `vintage` — four-digit year or `"NV"` (non-vintage)
- `grape` — must match an entry in `grapes.json` (either a TasteRank variety, a named blend, or an additional variety)
- `color` — one of `red`, `white`, `rosé`, `sparkling`
- `rating` — number, one decimal, 0–10
- `date_tasted` — ISO date `YYYY-MM-DD`
- `lat` / `lng` — taken from `regions.json` for the wine's region

## Adding a new region or grape

**New region:** edit `regions.json`, add an entry with the region name as key and `{"lat": …, "lng": …}` as value. Then you can use that region for a wine.

**New grape variety:** if the grape is one of the [101 TasteRank varieties](#), it's already in `grapes.json` — just use the name. If it's a new named blend or a variety outside the 101, add it to the relevant section of `grapes.json` first.

## Taxonomy philosophy

Wines are categorized by **a single grape/category field**, where grape varieties and named wine categories sit as peers — Option C in design terms. A Barolo is tagged `"Barolo"` (not decomposed to `"Nebbiolo"`). A Châteauneuf-du-Pape is tagged `"Châteauneuf-du-Pape Blend"` (not `"Grenache"`). A varietal Grenache from Gredos is tagged `"Grenache"`. This matches how sommeliers, wine lists, and retail actually organize wine — the named wine is its own identity, not a derivative of its dominant grape.

Borderline cases are handled individually. See the Phase 1 data report for the reasoning behind specific classification decisions.

## About

This project grew out of 20+ years of wine collecting and tasting. It is a personal reference tool — a codex of every wine I have notable memory of having tasted, organized for my own lookup and reflection.

The grape reference draws on my [TasteRank Grape Variety Reference](#), which provides one-paragraph profiles of 101 varieties.

## License

This work is licensed under [Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/). You are free to share and adapt this material for non-commercial purposes with attribution.

---

*Jure Skarabot · New York · 2026*
