---
dataset_id: bicing-barcelona-2019-03
source_url: https://opendata-ajuntament.barcelona.cat/data/en/dataset/bicing
download_date: 2026-04-21
license: CC BY 4.0 (Ajuntament de Barcelona — Open Data BCN)
update_cadence: historical (publication discontinued 2019-03-31)
format: CSV (comma-delimited, UTF-8, quoted strings)
time_coverage: 2019-03-01 → 2019-03-31
---

# Barcelona Bicing — March 2019

## What's in this directory

**IMPORTANT:** Trip-level Bicing data (start_station, end_station, duration per
rental) is NOT publicly available from Open Data BCN. The closest public data
is two aggregate views we download here — per-station snapshots (rich, big) and
system-wide usage (simple, small). Any lecture referencing "bike-sharing trips"
should use language like "bike-share station state" or "bike-share usage" to
stay accurate to what the data actually represents. See `README-usage.md`.

| File | Rows | Size | Purpose |
|------|------|------|---------|
| `station-snapshots-2019-03.csv` | 3,954,821 | ~370 MB | Primary dataset: per-station, per-minute state |
| `sample.csv` | 100,000 | ~9 MB | Reservoir sample (seed=42) of snapshots, for quick-inspection exercises |
| `system-usage-2019-03.csv` | 5,184 | ~160 KB | Contrasting small aggregate for size-comparison examples |
| `README-usage.md` | — | — | When to use which file + caveats |

## Primary dataset — `station-snapshots-2019-03.csv`

One row per station per timestamp (~1-minute cadence). ~400 stations × ~10 k
timestamps in a month = ~4 million rows.

### Schema

| Column | Type | Notes |
|--------|------|-------|
| `id` | | |
| `type` | | |
| `latitude` | | |
| `longitude` | | |
| `streetName` | | |
| `streetNumber` | | |
| `altitude` | | |
| `slots` | | |
| `bikes` | | |
| `nearbyStations` | | |
| `status` | | |
| `updateTime` | | |

Expected semantics (verify column names in the file before writing lecture examples):

- `id` — station identifier (integer 1 … ~400)
- `type` — `BIKE` for mechanical, `ELECTRIC_BIKE` for e-bikes (introduced March 2019)
- `latitude`, `longitude` — station location (floats)
- `streetName`, `streetNumber` — human-readable address
- `altitude` — metres above sea level (integer)
- `slots` — total docking points at the station
- `bikes` — bikes currently parked (≤ slots)
- `nearbyStations` — comma-separated list of ids of nearby stations
- `status` — `OPN` (open), `CLS` (closed), sometimes other codes
- `updateTime` — timestamp, European format `DD/MM/YY HH:MM:SS`

### Sample rows

```
"id","type","latitude","longitude","streetName","streetNumber","altitude","slots","bikes","nearbyStations","status","updateTime"
1,"BIKE",41.397952,2.180042,"Gran Via Corts Catalanes","760",21,29,0,"1","CLS","28/02/19 23:59:12"
2,"BIKE",41.39553,2.17706,"Roger de Flor/ Gran Vía","126",21,27,0,"2","CLS","28/02/19 23:59:12"
3,"BIKE",41.394055,2.181299,"Nàpols","82",22,27,0,"3","CLS","28/02/19 23:59:12"
```

## Secondary dataset — `system-usage-2019-03.csv`

One row per timestamp with system-wide totals. No station dimension.

### Schema

| Column | Type | Notes |
|--------|------|-------|
| `error` | | |
| `bikesInUsage` | | |
| `electricalBikesInUsage` | | |
| `mechanicalBikesInUsage` | | |
| `dateTime` | | |

### Sample rows

```
"error","bikesInUsage","electricalBikesInUsage","mechanicalBikesInUsage","dateTime"
0,47,4,43,"2019-02-28 23:59:57"
0,46,3,43,"2019-03-01 00:04:57"
0,40,2,38,"2019-03-01 00:09:57"
```

## Known quirks — the pedagogical gold

These are the real data-quality issues a learner will discover. Each maps
directly to a lecture:

- **European date format** (`28/02/19 23:59:12`) — `pd.to_datetime` with default
  settings misparses February 28 as February 19. Gold material for **story-3.2
  (type errors & outliers)**.
- **Two-digit year** (`19` not `2019`) — second gotcha on top of date parsing.
- **String columns with numeric content** — `streetNumber` is often stored as
  string (`"760"`, `"s/n"` for "no number"). Naïve `.describe()` or numeric
  aggregation silently drops these. **story-3.2** / **story-2.3 (inspection)**.
- **`bikes > slots`** — occasional data glitches where reported bikes exceeds
  dock capacity. Validation logic opportunity. **story-3.1 (missing values and
  anomalies)**.
- **Closed stations still appearing** — `status == "CLS"` rows coexist with
  `OPN` rows. Students filtering naively will double-count. **story-3.5 (EDA)**.
- **`nearbyStations`** is a quasi-list stored as a string — parsing it is an
  exercise in directing the LLM to handle structured-in-string data. **story-3.3
  (relational thinking)**.
- **Size** — 370 MB for one month is past "paste into LLM" territory, and an
  excellent anchor for **story-1.4 (context window)** and **story-2.5 (data
  format selection)**.

## Refresh instructions

```bash
python3 scripts/download-mobility-datasets/download.py --only bicing --force
```

Publication is discontinued (last file March 2019). `--force` is only needed to
regenerate `sample.csv` with a different seed.

## Lectures that reference this dataset

- story-2.2 (loading CSV) — use `sample.csv`
- story-2.3 (inspection) — use `sample.csv`; discover date-format, status codes, bikes > slots
- story-3.1 (missing values) — CLS vs. OPN status, missing coordinates
- story-3.2 (type errors & outliers) — European date format, string-stored numbers
- story-3.3 (relational structure) — `nearbyStations` in-string list
- story-3.5 (EDA) — peak hours computed from `updateTime`; busiest stations by `id` (run on the cleaned dataset)
- story-4.x (visualization) — station map using lat/lon; usage heatmap over time × station
