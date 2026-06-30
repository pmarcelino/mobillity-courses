# Bicing — which file for which exercise

## The trip-data caveat

Open Data BCN does **not** publish trip-level Bicing data. Every row in these
files is either a **per-station state snapshot** (`station-snapshots-*.csv`) or
a **system-wide aggregate** (`system-usage-*.csv`). No individual rentals.

When writing lectures:

- ✅ "bike-share station state", "bike-share usage", "occupancy over time"
- ✅ "peak hours" (derivable from the timestamps)
- ✅ "busiest stations" (derivable from how often a station has bikes < slots)
- ❌ "trip duration" — not in the data; do not fabricate it
- ❌ "origin-destination flows" — not in the data

If a lecture needs genuine trip-level data, MITMA Spain O-D
(`../../mitma-spain-od/`) is the correct source — it's per-trip O-D at district
level, nationally representative, and real.

## `sample.csv` (~100 k rows)

Reservoir-sampled from `station-snapshots-2019-03.csv` with seed 42.

Use for:
- First-inspection exercises (story-2.2 → 2.3)
- Iteration: editing LLM prompts without waiting for the full 4M-row file to load
- Token-budget demos (story-1.4): small enough to paste in an LLM chat

Preserves schema and column distributions of the full file; directional insights
on the sample hold on the full month.

## `station-snapshots-2019-03.csv` (~4M rows)

The real thing. Use for:
- Full cleaning-and-exploration pipeline (story-3.6)
- Demonstrating pandas speed on ~4M rows (story-2.5 motivation)
- Visualization exercises where density matters (story-4.x heatmaps, station maps)
- Any evaluation exercise where rare events matter (story-5.x)

## `system-usage-2019-03.csv` (~5 k rows)

A simple, small dataset. Use for:
- Contrast with the full snapshots file in story-1.4 / 2.5: "same system, two
  very different data shapes — which one belongs in an LLM context?"
- Introductory inspection exercises where the student should succeed trivially
  before moving to the messier file
