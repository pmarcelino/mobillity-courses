---
dataset_id: bicing-barcelona-synthetic-trips
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-3-lab.py (seeded builder; notebook 3.5 reads this CSV from the published course repo)
source_url: synthetic — no public trip-level Bicing data exists; this is fabricated
download_date: 2026-06-28
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-3-lab.py --csv <dir> (seed=42)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic March 2019
---

# Synthetic bike-share trips — `trips-sample.csv`

**Synthetic, fake-but-plausible** trip-level data (5,000 rows), already cleaned and
joined — the starting point for exploratory analysis. **Not real data.**

> **Why synthetic?** No public *trip-level* Bicing data exists (the real feed only
> publishes per-station snapshots and system-wide totals). Story-3.5 teaches EDA on
> trip-level data, so this table is fabricated to give that lecture something to explore.

Used by **story-3.5** (exploring data with LLMs).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `trip_id` | string | Synthetic trip identifier (`T100000`, `T100001`, …). |
| `start_time` | string | ISO `YYYY-MM-DD HH:MM:SS` trip start, across March 2019. |
| `start_station` | string | Origin station name. |
| `end_station` | string | Destination station name. |
| `duration_min` | int | Trip duration in minutes; longer on weekends. |

## Sample rows

```
trip_id,start_time,start_station,end_station,duration_min
T100000,2019-03-01 06:12:00,Carrer de Tarragona,Carrer de Sants,7
T100001,2019-03-01 06:30:00,Carrer de Muntaner,Carrer de Pau Claris,7
T100002,2019-03-01 06:36:00,Carrer de Provenca,Carrer de Pau Claris,10
```

## Known patterns — for EDA

This table is **clean on purpose** (no missing values, correct types) so story-3.5 can
focus on asking questions rather than cleaning. Seed-stable patterns:

- **Commute peaks** — trips concentrate at hours **8** and **18** on weekdays, so
  "group by hour of day and count" surfaces a clear morning/evening pattern. → **story-3.5**.
- **A few busy stations** — a handful of stations dominate at the peak window, so
  "filter to peak hours, group by station, sort" produces a meaningful ranking. → **story-3.5**.
- **Weekend vs weekday duration** — weekend trips are longer (mean ≈ 24 min) than weekday
  trips (mean ≈ 12 min), so the weekday/weekend duration comparison shows a real
  difference. → **story-3.5**.

## Regenerate

Run the module's seeded builder:

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-3-lab.py --csv <dir>
```

Seeded (`np.random.default_rng(42)`) → reproducible byte-for-byte. The notebook does **not**
generate the data — `notebook-3.5-trips.ipynb` **reads** this published copy from the course's
public repo (`…/barcelona-bicing-synthetic/trips-sample.csv`).
