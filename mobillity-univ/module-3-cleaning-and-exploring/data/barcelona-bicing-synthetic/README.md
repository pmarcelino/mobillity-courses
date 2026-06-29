# Synthetic Bicing data for the Module 3 Colab labs

This folder holds a **small, synthetic, fake-but-plausible** Barcelona-Bicing-style
dataset built specifically for the Module 3 (cleaning and exploring data) Colab notebooks under `docs/notebooks/` — one notebook per lecture (see the table below).

It is **not real data.** It exists because the real Bicing data is unsuitable for a Colab teaching notebook:

- the real per-station snapshots are ~370 MB (too heavy for Colab), and
- no public *trip-level* Bicing data exists at all.

Every row is fabricated, but the **columns, types, quirks, and numbers match what the Module 3 lecture scripts claim**, so the commands in those lectures run and produce the stated results. Each dataset is produced by a single **seeded** builder (`np.random.default_rng(42)`), so it reproduces byte-for-byte. The notebooks themselves do **not** generate data — they **read** the published CSV straight from this public GitHub repo.

## Files — one example notebook per lecture

Notebooks live in `docs/notebooks/`. Lectures 3.1 and 3.2 share one dataset but get a notebook each (one notebook per lecture).

| CSV | Rows | Card | Example notebook(s) (`docs/notebooks/`) | Lectures |
|-----|------|------|------------------------------------|----------|
| `station-snapshots-sample.csv` | 1,248 | [station-snapshots-card.md](./station-snapshots-card.md) | `notebook-3.1-handling-missing-values.ipynb`, `notebook-3.2-fixing-data-types.ipynb` | 3.1 missing values, 3.2 data types |
| `system-usage-sample.csv` | 5,184 | [system-usage-card.md](./system-usage-card.md) | `notebook-3.3-system-usage.ipynb` | 3.3 outliers |
| `trips-sample.csv` | 5,000 | [trips-card.md](./trips-card.md) | `notebook-3.5-trips.ipynb` | 3.5 EDA |

Each notebook is the **worked preview** of its lecture: it **reads** the CSV it needs from this repo (3.1 and 3.2 read the same one), then every command the lecture shows runs with a checkable printed result.

## Practice files (for the exercises)

The exercises use four smaller, **seed-7** practice CSVs, published alongside the worked CSVs and read by the learner's LLM prompt from their raw GitHub URL: `stations_practice.csv` (240 rows), `types_practice.csv` (240), `usage_practice.csv` (2,016), `trips_practice.csv` (2,000).

## Exercises (learner's turn)

One exercise per lecture, in `docs/exercises/`, each reading its published practice CSV from this repo and asking the learner to direct an LLM to write the code:

`exercise-3.1-handling-missing-values.md`, `exercise-3.2-fixing-data-types.md`,
`exercise-3.3-detecting-and-handling-outliers.md`, `exercise-3.5-exploring-data-with-llms.md`.

## Scope

Covers the four **Bicing-based** Module 3 stories: 3.1 (missing values), 3.2 (data types), 3.3 (outliers), 3.5 (EDA). The GTFS-based stories 3.4 and 3.6 use a different dataset family (multi-table transit feeds) and are out of scope for this synthetic bundle.

## Regenerate

Run the module's seeded builder (`courses/mobillity-univ/docs/notebooks/build-module-3-lab.py --csv <dir> --practice <dir>`): it writes the 3 worked CSVs and the 4 practice CSVs byte-for-byte (`np.random.default_rng(42)` / seed 7). The dataset cards are reconciled against these CSVs with `scripts/validate_examples/check_card.py`. These assets were produced by the `build-module-lab` skill.
