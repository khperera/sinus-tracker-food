# Sinus Tracker

Live app: **https://khperera.github.io/sinus-tracker-food/**

Tracks sinus pain levels and food/encounter exposures separately, with a zoomable absolute-time timeline to spot correlations.

## Features

- **Pain log** — 1–10 slider, log a reading now or backdate it ("30 min ago" → "12 hours ago"); optional note and secondary symptom scores (congestion, pressure, headache, post-nasal drip)
- **Exposure log** — add foods/encounters as chips, set how far back the time window goes (30 min → 12 hours); one-tap quick-log chips for your most frequent items; "did you mean…?" prompt and a merge tool keep trigger spellings consolidated
- **Medication stack, doses & schedules** — toggleable stack saved with each pain log; one-tap 💊 dose events on the timeline; per-med repeat cadence (daily → every 30 days) with "due" reminders, and optional auto-logging that backfills scheduled doses anchored to your last real one
- **Timeline chart** — scatter plot of pain over absolute time; dashed orange lines mark exposures, solid blue lines mark med doses, grey line shows the 24h rolling trend
- **Zoom & pan** — scroll to zoom the time axis, drag to pan, preset buttons (Today / 7d / 30d / All)
- **Trigger ↔ pain correlation** — paired analysis: each exposure is compared against itself (avg pain 24h after − 24h before), with sample sizes shown, low-n cells faded, and warnings when two triggers always co-occur
- **Hour × day heatmap** — average pain by hour of day and day of week, to surface circadian/weekly patterns
- **Weather & pollen capture (opt-in)** — snapshots local temperature, humidity, pressure, and pollen (Open-Meteo, keyless) with each pain log
- **Edit & delete** — fix or remove entries from the Recent Entries panel
- **Export / import** — pain CSV, exposure CSV, full JSON; JSON re-imports with merge-by-id (restore backups, move devices); printable doctor report
- **Durable, no server** — all data lives in your browser's localStorage; corrupt blobs are quarantined instead of overwritten, `storage.persist()` is requested, and a backup-age reminder nudges you to export. Installable as a PWA (offline-capable via service worker; home-screen install exempts iOS Safari's 7-day storage eviction)

## Enabling GitHub Pages

1. Go to **Settings → Pages** in this repository
2. Under *Source*, choose **Deploy from a branch**
3. Select branch **main**, folder **/ (root)**
4. Save — the site will be live at the URL above within a minute

## Data format

**Pain CSV**
```
timestamp,pain,congestion,sinus_pressure,headache,postnasal_drip,meds,note,temp_c,humidity_pct,pressure_hpa,pollen
2026-06-11T14:32:00.000Z,7,6,4,,,"flonase; zyrtec","woke up congested",21.4,48,1013,12
```

**Exposure CSV**
```
timestamp,hours_before,items
2026-06-11T11:00:00.000Z,4,"coffee,wine,dusty room"
```

**Full JSON** (schema v3 — re-importable with merge-by-id; medications carry `cadenceDays`/`autoLog`, auto-backfilled doses are flagged `"auto": true`)
```json
{
  "schemaVersion": 3,
  "exportedAt": "2026-06-11T14:32:00.000Z",
  "painLog": [],
  "exposureLog": [],
  "medications": [],
  "medDoses": []
}
```

Query examples:
- pandas: `df[df['pain'] >= 7]`
- jq: `jq '[.painLog[] | select(.pain >= 7)]' sinus-tracker-export.json`
