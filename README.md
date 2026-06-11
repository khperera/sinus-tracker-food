# Sinus Tracker

Live app: **https://khperera.github.io/sinus-tracker-food/**

Tracks sinus pain levels and food/encounter exposures separately, with a zoomable absolute-time timeline to spot correlations.

## Features

- **Pain log** — 1–10 slider, log a reading at any moment
- **Exposure log** — add foods/encounters as chips, set how far back the time window goes (30 min → 12 hours)
- **Timeline chart** — scatter plot of pain over absolute time; dashed vertical lines mark exposures with item labels
- **Zoom & pan** — scroll to zoom the time axis, drag to pan, preset buttons (Today / 7d / 30d / All)
- **Export** — download pain log as CSV, exposure log as CSV, or everything as JSON
- **No server** — all data lives in your browser's localStorage for the GitHub Pages domain

## Enabling GitHub Pages

1. Go to **Settings → Pages** in this repository
2. Under *Source*, choose **Deploy from a branch**
3. Select branch **main**, folder **/ (root)**
4. Save — the site will be live at the URL above within a minute

## Data format

**Pain CSV**
```
timestamp,pain
2026-06-11T14:32:00.000Z,7
```

**Exposure CSV**
```
timestamp,hours_before,items
2026-06-11T11:00:00.000Z,4,"coffee,wine,dusty room"
```

Query examples:
- pandas: `df[df['pain'] >= 7]`
- jq: `jq '[.painLog[] | select(.pain >= 7)]' sinus-tracker-export.json`
