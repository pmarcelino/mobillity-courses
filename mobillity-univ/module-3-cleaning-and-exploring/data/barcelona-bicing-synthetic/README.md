# Synthetic Bicing data for the Module 3 Colab labs (stories 3.1–3.3)

This folder holds a **small, synthetic, fake-but-plausible** Barcelona-Bicing-style
dataset built specifically for the Module 3 (cleaning and exploring data) Colab notebooks
under `docs/notebooks/` — one notebook per lecture (see the table below).

It is **not real data.** It exists because the real Bicing per-station snapshots are
~370 MB (too heavy for Colab), so this small seeded stand-in reproduces their columns,
types, and quirks instead.

Every row is fabricated, but the **columns, types, quirks, and numbers match what the
Module 3 lecture scripts claim**, so the commands in those lectures run and produce the
stated results. Each dataset is produced by a single **seeded** builder
(`docs/notebooks/build-module-3-lab.py`, `np.random.default_rng(42)`), so it reproduces
byte-for-byte. The notebooks themselves do **not** generate data — they **read** the
published CSV straight from the course's public GitHub repo.

> **Story 3.5 (trip-level EDA) moved to Porto.** Its `trips-sample.csv` /
> `trips_practice.csv` and card now live in the sibling **`porto-trips-synthetic/`**
> folder. This folder now covers only the snapshot/usage stories 3.1–3.3.

## Files — one example notebook per lecture

Notebooks live in `docs/notebooks/`. Lectures 3.1 and 3.2 share one dataset but get a
notebook each (one notebook per lecture).

| CSV | Rows | Card | Example notebook(s) (`docs/notebooks/`) | Lectures |
|-----|------|------|------------------------------------|----------|
| `station-snapshots-sample.csv` | 1,248 | [station-snapshots-card.md](./station-snapshots-card.md) | `notebook-3.1-handling-missing-values.ipynb`, `notebook-3.2-fixing-data-types.ipynb` | 3.1 missing values, 3.2 data types |
| `system-usage-sample.csv` | 5,184 | [system-usage-card.md](./system-usage-card.md) | `notebook-3.3-system-usage.ipynb` | 3.3 outliers |

Each notebook is the **worked preview** of its lecture: it **reads** the CSV it needs from
the published repo (3.1 and 3.2 read the same one), then every command the lecture shows runs
with a checkable printed result.

## Practice files (for the exercises)

The exercises use smaller, **seed-7** practice CSVs, published alongside the worked CSVs
and read by the learner's LLM prompt from their raw GitHub URL:
`stations_practice.csv` (240 rows), `types_practice.csv` (240), `usage_practice.csv` (2,016).
They are built by the same builder (`build-module-3-lab.py --practice <dir>`). (Story 3.5's
`trips_practice.csv` is in `porto-trips-synthetic/`.)

## Exercises (learner's turn)

One exercise per lecture, in `docs/exercises/`, each reading its published practice CSV from
the course repo and asking the learner to direct an LLM to write the code:
`exercise-3.1-handling-missing-values.md`, `exercise-3.2-fixing-data-types.md`,
`exercise-3.3-detecting-and-handling-outliers.md`. (Story 3.5's exercise,
`exercise-3.5-exploring-data-with-llms.md`, reads from `porto-trips-synthetic/`.)

## Scope

Covers the three snapshot/usage Module 3 stories: 3.1 (missing values), 3.2 (data types),
3.3 (outliers). Story 3.5 (EDA) moved to Porto (`porto-trips-synthetic/`). The GTFS-based
stories 3.4 and 3.6 use a different dataset family (multi-table transit feeds) and are out of
scope for this synthetic bundle.

## Regenerate

Run the module's seeded builder:

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-3-lab.py --csv <dir> --practice <dir>
```

It writes the 2 worked snapshot/usage CSVs (`--csv`) and 3 practice CSVs (`--practice`)
byte-for-byte here (`np.random.default_rng(42)` / seed 7), and the Porto trips (worked +
practice) into the sibling `porto-trips-synthetic/` folder. The dataset cards are reconciled
against these CSVs with `scripts/validate_examples/check_card.py`. These assets were produced
by the `build-module-lab` skill.

> **Relationship to the real data:** the real (large) Bicing dataset lives in
> `../barcelona-bicing/`. This synthetic folder mirrors its schema for teaching only and
> does not replace it.
