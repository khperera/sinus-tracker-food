# Data logging / storage / presentation — optimization plan

Ordered by value-per-effort, from the design review.

## 1. Storage durability & integrity
- [x] Collision-proof entry ids (`crypto.randomUUID()`), migrate legacy entries on load
- [x] Corruption-safe loaders — preserve a bad blob under a `*_corrupt_*` key instead of letting the next save overwrite history
- [x] JSON **import** with merge-by-id (restore a backup, move devices, poor-man's sync)
- [x] Schema version + `exportedAt` in the JSON export
- [x] `navigator.storage.persist()` + backup-age status line in a Data & Backup card
- [x] PWA: manifest, icons, service worker (offline app shell + CDN cache) so iOS home-screen installs escape Safari's 7-day storage eviction
- [x] Multi-tab safety: re-render on `storage` events from other tabs

## 2. Entry correctness
- [x] Backdate pain logs ("Now / 30 min ago / … / 12 hours ago" selector)
- [x] Edit & delete entries from Recent Entries (pain value edit, delete for all types)
- [x] Store `hoursBefore` on exposure entries and export it in the CSV (matches README, preserves timing uncertainty)
- [x] "Did you mean…?" merge prompt when a new item is within edit-distance 2 of an existing one (stops trigger fragmentation: coffee / coffe / iced coffee)

## 3. Honest correlation analysis
- [x] Paired before/after deltas: for each exposure, mean pain in 24h after − 24h before (each exposure is its own control)
- [x] Min-n gating: fade cells backed by <3 readings, show n everywhere
- [x] Co-occurrence warnings when a trigger is logged with the same companion ≥80% of the time

## 4. Medication doses as events
- [x] One-tap 💊 dose logging per med (timestamped event, separate from the active-stack snapshot)
- [x] Dose markers on the timeline chart
- [x] Doses in Recent Entries, JSON export/import, and doctor report

## 5. Logging ergonomics & chart readability
- [x] Quick-log chips for the most frequent exposure items (one tap = logged)
- [x] 24h centered rolling-average trend line on the chart

## 6. Context & patterns
- [x] Optional weather + pollen capture per pain log (Open-Meteo, keyless; opt-in, uses device location)
- [x] Hour-of-day × day-of-week pain heatmap
- [x] Printable doctor report (summary stats, suspected triggers with n, med history)

## Deferred (not in this pass)
- Multi-symptom scores (pressure / congestion / headache / drip) — schema change, UI weight
- Per-item timestamps within one exposure batch — quick-log chips cover most of the need
- IndexedDB migration — localStorage volume is tiny; durability work above matters more
- Cloud sync — export/import-with-merge is the offline-first version of this
