# Synthetic Bicing data for the Module 4 visualization exercises (stories 4.2–4.5)

This folder holds **small, synthetic, fake-but-plausible** Barcelona-Bicing-style
datasets built specifically for the Module 4 (visualizing results) Colab exercises
under `docs/exercises/` — one exercise per lecture (see the table below).

It is **not real data.** It exists because the real Bicing snapshots are ~370 MB (too
heavy for Colab), so these small seeded stand-ins reproduce their columns and types
instead, with patterns planted so each lecture's chart has a payoff.

Every row is fabricated, but the **columns and types match the real
`bicing-barcelona-2019-03` dataset card**, and the planted patterns match what the
Module 4 exercises ask learners to find. Each dataset is produced by a single **seeded**
builder (`docs/notebooks/build-module-4-lab.py`, `np.random.default_rng(7)`), so it
reproduces byte-for-byte. The exercises do **not** generate data — the learner's LLM
prompt **reads** each published CSV straight from the course's public GitHub repo.

> **Practice CSVs only (for now).** The worked `-sample.csv` files and the per-lecture
> example notebooks arrive with the full Module 4 lab build; when they do, these
> practice generators must not change (the exercises' published "check your work"
> numbers depend on byte-identity). Story 4.5's choropleth stretch data lives in the
> sibling **`mitma-spain-od-synthetic/`** folder.

## Files — one practice CSV per exercise

| CSV | Rows | Card | Exercise (`docs/exercises/`) | Lecture topic |
|-----|------|------|------------------------------|---------------|
| `usage_week_practice.csv` | 672 | [usage-week-card.md](./usage-week-card.md) | `exercise-4.2-static-charts-with-matplotlib.md` | 4.2 static charts, draft → refine |
| `snapshots_types_practice.csv` | 1,200 | [snapshots-types-card.md](./snapshots-types-card.md) | `exercise-4.3-statistical-plots-with-seaborn.md` | 4.3 what averages hide |
| `usage_march_practice.csv` | 1,344 | [usage-march-card.md](./usage-march-card.md) | `exercise-4.4-interactive-charts-with-plotly.md` | 4.4 zoom + hover payoff |
| `stations_map_practice.csv` | 60 | [stations-map-card.md](./stations-map-card.md) | `exercise-4.5-geographic-visualization-with-plotly.md` | 4.5 point map of stations |

## Regenerate

Run the module's seeded builder:

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

It writes these 4 practice CSVs (seed 7) byte-for-byte here, and the province flows CSV
into the sibling `mitma-spain-od-synthetic/` folder. The dataset cards are reconciled
against these CSVs with `scripts/validate_examples/check_card.py`. These assets were
produced by the `build-module-lab` skill (exercise-only scope).

> **Relationship to the real data:** the real (large) Bicing dataset lives in
> `../barcelona-bicing/`; the Module 3 cleaning-focused synthetic bundle lives in
> `../barcelona-bicing-synthetic/` (canonical to `build-module-3-lab.py`). This folder
> mirrors the same real schema for **visualization** teaching only and does not replace
> either.
