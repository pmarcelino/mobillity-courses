---
dataset_id: bicing-barcelona-synthetic-stations-map
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-4-lab.py (seeded builder; exercise-4.5 reads this CSV from the published course repo)
source_url: synthetic — fabricated to match the schema of bicing-barcelona-2019-03 station snapshots
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-4-lab.py --practice <dir> (seed=7)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic — one row per station, no time dimension
---

# Synthetic Bicing station locations — `stations_map_practice.csv`

**Synthetic, fake-but-plausible** station directory (60 rows, one per station), a column
subset of the real `station-snapshots-2019-03.csv` schema with **every coordinate
present and valid** — this is a mapping exercise, not a cleaning one. Coordinates are
fabricated but fall inside the real Barcelona Bicing service area. **Not real data.**

Used by **exercise-4.5** (geographic visualization with plotly: a point map of stations).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `id` | int | Station identifier, 1–60. |
| `type` | string | `BIKE` (exactly 45 stations) or `ELECTRIC_BIKE` (exactly 15) — the map's colour split. |
| `latitude` | float | Station latitude, 41.368–41.412; complete, no missing values. |
| `longitude` | float | Station longitude, 2.135–2.197; complete, no missing values. |
| `streetName` | string | Human-readable street name — the hover label. |
| `slots` | int | Total docking points; one unique biggest station (42). |
| `bikes` | int | Bikes currently parked. |

## Sample rows

```
id,type,latitude,longitude,streetName,slots,bikes
1,BIKE,41.386007,2.160819,Via Laietana,34,24
2,BIKE,41.387792,2.167133,Carrer de Tarragona,29,24
3,BIKE,41.384355,2.164129,Passeig de Sant Joan,27,20
```

## Known quirks — the pedagogical gold

Seed-stable (seed 7); the exercise's "check your work" numbers depend on them:

- **Three visible clusters** — 30 stations around the downtown core (≈ 41.386, 2.170),
  20 to the north-east (≈ 41.404, 2.191), 10 to the south-west (≈ 41.373, 2.140). The
  dots form obvious groups the moment they land on a street map. → exercise-4.5.
- **Deterministic colour split** — exactly **45 `BIKE` / 15 `ELECTRIC_BIKE`**, so
  colouring by type is checkable by count. → exercise-4.5.
- **One landmark station** — station **17, Passeig de Gracia, 42 slots** (every other
  station ≤ 36): the biggest dot/hover target the check lands on. → exercise-4.5.
- **Clean coordinates** — zero missing values; nothing to drop before mapping.

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

Seeded (`np.random.default_rng(7)`) → reproducible byte-for-byte. The exercise does
**not** generate the data — the learner's LLM prompt **reads** this published copy from
the course's public repo (`…/module-4-visualizing/data/barcelona-bicing-viz-synthetic/stations_map_practice.csv`).
