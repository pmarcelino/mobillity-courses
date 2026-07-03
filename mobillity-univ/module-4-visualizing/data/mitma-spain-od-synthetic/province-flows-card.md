---
dataset_id: mitma-spain-od-synthetic-province-flows
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/build-module-4-lab.py (seeded builder; exercise-4.5's choropleth stretch reads this CSV from the published course repo)
source_url: synthetic — fabricated to match the column semantics of mitma-spain-od-v2 (basic-daily Viajes distritos)
download_date: 2026-07-03
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate with build-module-4-lab.py --practice <dir> (seed=7)
format: CSV (comma-delimited, UTF-8 — the REAL MITMA files are pipe-delimited .csv.gz; this teaching stand-in is a plain comma CSV)
time_coverage: synthetic single day labelled 20250303
---

# Synthetic Spain district-to-district flows — `province_flows_practice.csv`

**Synthetic, fake-but-plausible** one-day origin-destination flow table (347 rows), a
column subset of the real MITMA `20250303_Viajes_distritos.csv.gz` semantics. Built for
one job: a **choropleth of trips by origin province** joined to the public
Spain-provinces GeoJSON. **Not real data** — the real file is ~203 MB compressed.

Used by **exercise-4.5**'s "going further" stretch (choropleth with an external GeoJSON).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `fecha` | int | Date label `YYYYMMDD`; constant `20250303` in this teaching set. |
| `origen` | string | 5-char **zero-padded** origin district code; the **first two digits are the INE province code** (`01`–`52`) — the choropleth join key. A default load reads it as an integer and silently destroys the leading zero. |
| `destino` | string | 5-char zero-padded destination district code, same format as `origen`. |
| `viajes` | float | Estimated trips (population-reweighted, hence non-integer, like the real MITMA values). |

## Sample rows

```
fecha,origen,destino,viajes
20250303,01187,36269,725.035
20250303,01250,12017,801.372
20250303,01090,48002,760.558
```

## Known quirks — the pedagogical gold

Seed-stable (seed 7); the exercise's "check your work" numbers depend on them:

- **The leading-zero trap** — `origen` values like `01187` must be read **as text**: a
  default numeric load turns `01187` into `1187`, and provinces `01`–`09` then never
  match the GeoJSON's `cod_prov` (zero-padded 2-char strings like `"01"`). The public
  Spain-provinces GeoJSON (`click_that_hood` … `spain-provinces.geojson`, 52 features)
  is the join target. → exercise-4.5 stretch (the prompt directs reading codes as text).
- **Full national coverage** — all **52 provinces** appear as origins (≥ 5 rows each),
  so the whole map shades; no grey holes to mistake for bugs.
- **Madrid darkest, Barcelona second** — total `viajes` by origin province: Madrid (28)
  ≈ **36,015**, Barcelona (08) ≈ **32,850**, third place ≈ 5,052. The two metropolises
  dominate by design, mirroring the real file's headline pattern.
- **Floats, not counts** — `viajes` is a reweighted estimate; summing is fine, rounding
  individual rows to "people" is not (same caveat as the real dataset).

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

Seeded (`np.random.default_rng(7)`) → reproducible byte-for-byte. The exercise does
**not** generate the data — the learner's LLM prompt **reads** this published copy from
the course's public repo (`…/module-4-visualizing/data/mitma-spain-od-synthetic/province_flows_practice.csv`).
