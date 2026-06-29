---
dataset_id: bicing-barcelona-synthetic-snapshots
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/notebook-3.1-handling-missing-values.ipynb and notebook-3.2-fixing-data-types.ipynb (identical Section 0)
source_url: synthetic — fabricated to match the schema and quirks of bicing-barcelona-2019-03
download_date: 2026-06-28
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate by re-running the notebook (seed=42)
format: CSV (comma-delimited, UTF-8, quoted list fields)
time_coverage: synthetic March 2019
---

# Synthetic Bicing station snapshots — `station-snapshots-sample.csv`

**Synthetic, fake-but-plausible** per-station snapshots, sized for Google Colab
(1,248 rows). It reproduces the columns, types, and data-quality quirks of the real
`bicing-barcelona-2019-03` station snapshots so the Module 3 lecture examples run
exactly as written. **Not real data.** Regenerate with the Module 3 notebook (seeded).

Used by **story-3.1** (handling missing values) and **story-3.2** (fixing data types).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `id` | int | Station identifier, 1–52. |
| `type` | string | `BIKE` (mechanical) or `ELECTRIC_BIKE`. |
| `latitude` | float (blank when missing) | Station latitude; blank in some rows — part of the missing-coordinate demo. |
| `longitude` | float (blank when missing) | Station longitude; blank wherever a coordinate is missing. |
| `streetName` | string | Human-readable street name. |
| `streetNumber` | string | **Stored as text.** Mostly numeric (`"161"`) but holds sentinels `s/n`, `SN`, and ranges like `23-25`. |
| `altitude` | int | Metres above sea level. |
| `slots` | int | Total docking points at the station. |
| `bikes` | int | Bikes currently parked; a few rows exceed `slots` (data glitch). |
| `nearbyStations` | string | Comma-separated list of nearby station ids, stored as text; blank in some rows. |
| `status` | string | All rows are `OPN` (open) in this teaching set. The real Bicing data also carries a closed-station code, which was omitted from the Module 3 materials. |
| `updateTime` | string | Timestamp in **European** `DD/MM/YY HH:MM:SS` text format. |

## Sample rows

```
id,type,latitude,longitude,streetName,streetNumber,altitude,slots,bikes,nearbyStations,status,updateTime
1,BIKE,41.437396,2.182954,Napols,161,35,35,6,"35,19",OPN,01/03/19 08:25:41
1,BIKE,41.437396,2.182954,Napols,15,35,35,33,"9,13,39",OPN,16/03/19 19:02:24
3,BIKE,,2.176413,Passeig de Gracia,85,35,24,19,"21,20,50,5",OPN,01/03/19 12:42:43
2,BIKE,41.403888,2.173188,Carrer de Balmes,s/n,94,21,4,"7,37,12",OPN,01/03/19 08:25:39
2,BIKE,41.403888,2.173188,Carrer de Balmes,291,94,21,1,,OPN,13/03/19 23:45:57
```

(Rows 3–5 show a blank `latitude`, an `s/n` `streetNumber`, and a blank `nearbyStations`.)

## Known quirks — the pedagogical gold

Counts are exact and seed-stable:

- **Missing coordinates** — 17 rows have a blank `latitude` and/or `longitude`. Dropping
  rows with a missing coordinate takes the table from **1,248 → 1,231** with **0**
  remaining nulls. → **story-3.1** (drop strategy, before/after verification).
- **Three forms of missingness** — true NaN (coordinates), empty string (22 blank
  `nearbyStations`), and a sentinel label (30 `s/n` in `streetNumber`). → **story-3.1**.
- **Numbers stored as text** — `streetNumber` is text; `pd.to_numeric(errors='coerce')`
  turns the 37 non-numeric tokens (`s/n`, `SN`, `23-25`) into NaN for inspection. →
  **story-3.2**.
- **European date text** — `updateTime` is `DD/MM/YY HH:MM:SS` text. A naive
  "first two characters are the hour" grabs the day-of-month and reports the busiest hour
  as `01`; `pd.to_datetime(format='%d/%m/%y %H:%M:%S')` then `.dt.hour` gives the correct
  peak (8). → **story-3.2**.
- **List-in-string** — `nearbyStations` is a quasi-list stored as text; split/normalize to
  use it. → **story-3.2**.
- **`bikes > slots`** — a few glitch rows where parked bikes exceed dock capacity.

## Regenerate

Open `courses/mobillity-univ/docs/notebooks/notebook-3.1-handling-missing-values.ipynb`
(or `notebook-3.2-fixing-data-types.ipynb` — their Section 0 is identical) and run
**Section 0**. The generator is seeded (`np.random.default_rng(42)`), so the file is
reproduced byte-for-byte every time.
