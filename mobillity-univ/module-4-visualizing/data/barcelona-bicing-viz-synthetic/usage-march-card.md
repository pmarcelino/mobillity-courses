---
dataset_id: bicing-barcelona-synthetic-usage-march
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-4-lab.py (seeded builder; exercise-4.4 reads this CSV from the published course repo)
source_url: synthetic — fabricated to match the schema of bicing-barcelona-2019-03 system-usage
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-4-lab.py --practice <dir> (seed=7)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic 2019-03-01 → 2019-03-28, 30-minute cadence
---

# Synthetic Bicing usage, four weeks — `usage_march_practice.csv`

**Synthetic, fake-but-plausible** system-wide usage time series (1,344 rows, one row per
30 minutes across four weeks). It reproduces the schema of the real
`system-usage-2019-03.csv` with a weekly rhythm plus **one planted surge** that only
zooming and hovering can pin down — the payoff of interactive charts. **Not real data.**

Used by **exercise-4.4** (interactive charts with plotly: zoom, hover, and extending the
hover with more context).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `error` | int | Sensor error flag; 0 in every row of this teaching set. |
| `bikesInUsage` | int | System-wide bikes in use. Weekly rhythm + one Friday-evening surge. |
| `electricalBikesInUsage` | int | E-bikes in use (~12% of `bikesInUsage`); the hover-extension column. |
| `mechanicalBikesInUsage` | int | Mechanical bikes in use (`bikesInUsage` − electrical). |
| `dateTime` | string | ISO `YYYY-MM-DD HH:MM:SS`, 30-minute cadence, 2019-03-01 → 2019-03-28. |

## Sample rows

```
error,bikesInUsage,electricalBikesInUsage,mechanicalBikesInUsage,dateTime
0,25,3,22,2019-03-01 00:00:00
0,29,3,26,2019-03-01 00:30:00
0,17,2,15,2019-03-01 01:00:00
```

## Known quirks — the pedagogical gold

Seed-stable (seed 7); the exercise's "check your work" numbers depend on them:

- **One Friday-evening surge** — the global maximum is **705** bikes at
  **2019-03-22 21:30** (the 21:00 point just below at 688). No other day exceeds 348, so
  zoomed out the spike is a blip; zooming into that week and hovering reveals the exact
  timestamp and value. → exercise-4.4 (the reason interactivity earns its place).
- **Weekly rhythm** — weekends run at ~0.55× weekday levels (weekday mean ≈ 153,
  weekend ≈ 83), a repeating four-week pattern that is obvious at full zoom. →
  exercise-4.4 (what the full view shows before zooming).
- **Electric share ≈ 12%** — steady at the surge too (12.1% at the peak point), so the
  extended hover answers "was the surge electric-driven?" with a clear *no, proportional*.
  → exercise-4.4 (the hover-extension prompt).

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

Seeded (`np.random.default_rng(7)`) → reproducible byte-for-byte. The exercise does
**not** generate the data — the learner's LLM prompt **reads** this published copy from
the course's public repo (`…/module-4-visualizing/data/barcelona-bicing-viz-synthetic/usage_march_practice.csv`).
