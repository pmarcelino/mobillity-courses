---
dataset_id: bicing-barcelona-synthetic-snapshots-types
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-4-lab.py (seeded builder; exercise-4.3 reads this CSV from the published course repo)
source_url: synthetic — fabricated to match the schema of bicing-barcelona-2019-03 station snapshots
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-4-lab.py --practice <dir> (seed=7)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic 2019-03-01 → 2019-03-14, service hours 06:00–22:59
---

# Synthetic Bicing snapshots by bike type — `snapshots_types_practice.csv`

**Synthetic, fake-but-plausible** per-station snapshots (1,200 rows = 30 stations ×
40 snapshots), a column subset of the real `station-snapshots-2019-03.csv` schema.
Engineered so that the two bike types share an **average** but hide completely
different **distributions** — the payoff of statistical plots. **Not real data.**

Used by **exercise-4.3** (statistical plots with seaborn: distribution comparison and
correlation heatmap).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `id` | int | Station identifier, 1–30. |
| `type` | string | `BIKE` (mechanical, 22 stations / 880 rows) or `ELECTRIC_BIKE` (8 stations / 320 rows). |
| `latitude` | float | Station latitude; complete, no missing values. |
| `longitude` | float | Station longitude; complete, no missing values. |
| `slots` | int | Total docking points; station 5 is the unique biggest (42). |
| `bikes` | int | Bikes currently parked. Occupancy = `bikes` ÷ `slots`. |
| `updateTime` | string | Timestamp in **European** `DD/MM/YY HH:MM:SS` text format. |

## Sample rows

```
id,type,latitude,longitude,slots,bikes,updateTime
1,BIKE,41.42251,2.15975,26,13,06/03/19 15:26:02
1,BIKE,41.42251,2.15975,26,18,08/03/19 06:05:41
1,BIKE,41.42251,2.15975,26,21,11/03/19 09:21:46
```

## Known quirks — the pedagogical gold

Seed-stable (seed 7); the exercise's "check your work" numbers depend on them:

- **Same middle, different spread** — occupancy (`bikes`/`slots`) has a median of
  ≈ **0.55 for both types** (0.545 vs 0.554) and near-identical means, but the standard
  deviations are ≈ **0.14 vs 0.30**: mechanical stations hover near the middle, electric
  stations are bimodal (nearly full or nearly empty). A bar chart of averages shows
  nothing; a violin/box plot shows everything. → exercise-4.3.
- **Occupancy falls through the day** — corr(occupancy, hour of day) ≈ **−0.61**, the
  standout cell in a correlation heatmap; day-of-week correlates with nothing (≈ 0.05),
  the designed contrast. → exercise-4.3.
- **European date text** — `updateTime` is `DD/MM/YY HH:MM:SS` **text**; the hour must be
  parsed day-first before any time-based analysis. → exercise-4.3 (the prompt states the
  format; a default parse reads it month-first and scrambles the dates without erroring).
- **Station 5 is the planted busiest** — unique 42 slots, highest mean `bikes` (≈ 22.0,
  next ≈ 20.5). Powers the boxplot-across-busiest-stations stretch prompt.
- **Clean otherwise** — no missing coordinates, no sentinel values; this is a plotting
  exercise, not a cleaning one.

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

Seeded (`np.random.default_rng(7)`) → reproducible byte-for-byte. The exercise does
**not** generate the data — the learner's LLM prompt **reads** this published copy from
the course's public repo (`…/module-4-visualizing/data/barcelona-bicing-viz-synthetic/snapshots_types_practice.csv`).
