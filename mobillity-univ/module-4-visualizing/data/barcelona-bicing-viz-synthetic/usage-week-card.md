---
dataset_id: bicing-barcelona-synthetic-usage-week
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-4-lab.py (seeded builder; exercise-4.2 reads this CSV from the published course repo)
source_url: synthetic — fabricated to match the schema of bicing-barcelona-2019-03 system-usage
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-4-lab.py --practice <dir> (seed=7)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic 2019-03-04 (Mon) → 2019-03-10 (Sun), 15-minute cadence
---

# Synthetic Bicing usage, one commute week — `usage_week_practice.csv`

**Synthetic, fake-but-plausible** system-wide usage time series (672 rows, one row per
15 minutes across one Monday-to-Sunday week). It reproduces the schema of the real
`system-usage-2019-03.csv` with a clean commute rhythm and **no planted outliers** —
this file exists so a bar chart of hourly averages is worth drawing and refining.
**Not real data.**

Used by **exercise-4.2** (static charts with matplotlib: draft → refine through
natural-language feedback).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `error` | int | Sensor error flag; 0 in every row of this teaching set. |
| `bikesInUsage` | int | System-wide bikes in use. Clean commute rhythm, no anomalies. |
| `electricalBikesInUsage` | int | E-bikes in use (~12% of `bikesInUsage`). |
| `mechanicalBikesInUsage` | int | Mechanical bikes in use (`bikesInUsage` − electrical). |
| `dateTime` | string | ISO `YYYY-MM-DD HH:MM:SS`, 15-minute cadence, 2019-03-04 → 2019-03-10. |

## Sample rows

```
error,bikesInUsage,electricalBikesInUsage,mechanicalBikesInUsage,dateTime
0,25,3,22,2019-03-04 00:00:00
0,28,3,25,2019-03-04 00:15:00
0,22,3,19,2019-03-04 00:30:00
```

## Known quirks — the pedagogical gold

Seed-stable (seed 7); the exercise's "check your work" numbers depend on them:

- **Evening peak tallest** — averaging `bikesInUsage` by hour of day puts hour **18**
  on top (average ≈ **284**), with the morning commute hour **8** second (≈ **257**).
  → exercise-4.2 (the bar chart has an unambiguous story to sharpen).
- **Overall average ≈ 133** — a horizontal reference line at the overall mean sits
  well below the commute peaks; 14 of the 24 hourly bars clear it. → exercise-4.2
  (the third refinement round adds this line).
- **Quiet nights** — hour 2 is the quietest (average ≈ 14), so the day shape reads
  instantly. Weekends run at ~0.6× weekday levels (weekday mean ≈ 151, weekend ≈ 90),
  which powers the weekday-vs-weekend stretch prompt.
- **No outliers** — unlike the Module 3 outlier-hunting series, nothing is planted to
  distract from chart-making.

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

Seeded (`np.random.default_rng(7)`) → reproducible byte-for-byte. The exercise does
**not** generate the data — the learner's LLM prompt **reads** this published copy from
the course's public repo (`…/module-4-visualizing/data/barcelona-bicing-viz-synthetic/usage_week_practice.csv`).
