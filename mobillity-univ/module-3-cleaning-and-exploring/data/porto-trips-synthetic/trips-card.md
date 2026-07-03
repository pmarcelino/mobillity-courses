---
dataset_id: porto-trips-synthetic
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-3-lab.py (seeded builder; notebook 3.5 reads this CSV from the published course repo)
source_url: synthetic — no public trip-level bike-share data exists, so this is fabricated
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-3-lab.py --csv <dir> (seed=42; trips land in the sibling porto-trips-synthetic/ folder)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic March 2019
---

# Synthetic Porto bike-share trips — `trips-sample.csv`

**Synthetic, fake-but-plausible** trip-level data (313 rows), already cleaned and
joined — the starting point for exploratory analysis. **Not real data.**

> **Why synthetic?** No public *trip-level* bike-share data exists (feeds publish
> per-station snapshots and system-wide totals, not individual trips). Story-3.5 teaches
> EDA on trip-level data, so this table is fabricated to give that lecture something to
> explore. The station names are real Porto metro stops (Aliados, Trindade, Casa da
> Música, Campanhã, …); the trips themselves are invented.

Used by **story-3.5** (exploring data with LLMs).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `trip_id` | string | Synthetic trip identifier (`T100000`, `T100001`, …). |
| `start_time` | string | ISO `YYYY-MM-DD HH:MM:SS` trip start, across March 2019. |
| `start_station` | string | Origin station (Porto metro stop name). |
| `end_station` | string | Destination station (Porto metro stop name). |
| `duration_min` | int | Trip duration in minutes; longer on weekends. |

## Sample rows

```
trip_id,start_time,start_station,end_station,duration_min
T100000,2019-03-01 06:36:00,Trindade,São Bento,15
T100001,2019-03-01 07:05:00,Trindade,Trindade,8
T100002,2019-03-01 07:07:00,Aliados,Campanhã,9
```

## Known patterns — for EDA

This table is **clean on purpose** (no missing values, correct types) so story-3.5 can
focus on asking questions rather than cleaning. The patterns below are **count-pinned**
in the seeded builder, so the notebook reproduces the lecture's dashboard numbers exactly:

- **Volume** — **313** trips total.
- **Commute peaks** — trips concentrate at hours **8** and **18** on weekdays: a morning
  peak of **08:00 (42 trips)** and a slightly higher evening peak of **18:00 (45 trips)**,
  so "group by hour of day and count" surfaces a clear morning/evening pattern. → **story-3.5**.
- **A few busy stations** — usage concentrates on a handful of stations. The four busiest
  are **Aliados (150)**, **Trindade (95)**, **Casa da Música (20)** and **Campanhã (15)** —
  **280** trips between them, with Aliados alone at **53.6%** and the top two at **87.5%**
  of that total. Three minor stations (Bolhão, São Bento, Marquês) hold the other 33 trips,
  so "count trips per station, sort, take the top four" produces a meaningful ranking.
  → **story-3.5**.
- **Weekend vs weekday duration** — weekend trips are longer (mean ≈ **23 min**) than
  weekday trips (mean ≈ **12 min**), so the weekday/weekend duration comparison shows a
  real difference. → **story-3.5**.

## Regenerate

Run the module's seeded builder:

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-3-lab.py --csv <barcelona-bicing-synthetic dir> --practice <barcelona-bicing-synthetic dir>
```

Seeded (`np.random.default_rng(42)` worked / seed 7 practice) → reproducible byte-for-byte.
The trips CSVs are written into a sibling **`porto-trips-synthetic/`** folder next to the
Barcelona snapshots/usage folder passed to `--csv`. The notebook does **not** generate the
data — `notebook-3.5-trips.ipynb` **reads** this published copy from the course's public
repo (`…/porto-trips-synthetic/trips-sample.csv`).
