# Synthetic Porto trips for the Module 3 EDA lab (story 3.5)

This folder holds a **small, synthetic, fake-but-plausible** table of **Porto** bike-share
trips, built specifically for the Module 3 lecture on **exploring data with an LLM** (story
3.5) — its Colab notebook (`docs/notebooks/notebook-3.5-trips.ipynb`) and its exercise
(`docs/exercises/exercise-3.5-exploring-data-with-llms.md`).

It is **not real data.** No public *trip-level* bike-share data exists — feeds publish
per-station snapshots and system-wide totals, not individual trips — so this table is
fabricated to give the EDA lecture something to explore. The station names are real Porto
metro stops (Aliados, Trindade, Casa da Música, Campanhã, and a few smaller ones); the trips
are invented. Rows are produced by the module's single **seeded** builder
(`np.random.default_rng(42)` worked / seed 7 practice), so they reproduce byte-for-byte, and
the counts are **pinned** so the notebook reproduces the lecture's dashboard numbers exactly.

The notebook does **not** generate the data — it **reads** the published CSV straight from
this public GitHub repo.

## Files

| CSV | Rows | Card | Read by | Lecture |
|-----|------|------|---------|---------|
| `trips-sample.csv` | 313 | [trips-card.md](./trips-card.md) | `notebook-3.5-trips.ipynb` (worked preview) | 3.5 EDA |
| `trips_practice.csv` | 2,000 | — | the learner's LLM prompt in `exercise-3.5-exploring-data-with-llms.md` | 3.5 EDA |

The practice CSV is deliberately larger and **seed-7** so the exercise's answers differ from
the notebook's — the learner explores rather than copies, but lands on the same story
(Aliados busiest, a morning commute peak, longer weekend trips).

## Known numbers (worked set)

- **313** trips; morning peak **08:00 (42)**, evening peak **18:00 (45)**, busiest hour 18.
- Busiest stations: **Aliados 150 · Trindade 95 · Casa da Música 20 · Campanhã 15** (280
  between them → Aliados **53.6%**, top-2 **87.5%**).
- Weekend trips ≈ **23 min** vs weekday ≈ **12 min**.

## Note

This trips table used to live in the sibling `barcelona-bicing-synthetic/` folder (Barcelona
stations). Story 3.5 moved to Porto to match its lecture video, so the trips now live here.
The other Module 3 lab datasets (per-station snapshots for 3.1/3.2, system usage for 3.3)
remain Barcelona and stay in `barcelona-bicing-synthetic/`.

## Regenerate

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-3-lab.py \
  --csv <…/data/barcelona-bicing-synthetic> --practice <…/data/barcelona-bicing-synthetic>
```

The trips (worked + practice) are written into this `porto-trips-synthetic/` folder — a
sibling of the Barcelona folder passed to `--csv`/`--practice`. The card is reconciled
against the CSV with `scripts/validate_examples/check_card.py`.
