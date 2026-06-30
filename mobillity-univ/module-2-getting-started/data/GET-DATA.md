# Getting the full Barcelona Bicing data

This folder ships `barcelona-bicing/sample.csv` (~9 MB) — enough to practice loading
and inspection. The **full** per-station snapshot for March 2019
(`station-snapshots-2019-03.csv`, ~353 MB) is too large for GitHub, so it is not bundled.

## Source
Barcelona Open Data — Bicing:
https://opendata-ajuntament.barcelona.cat/data/en/dataset/bicing

## Easiest way to get it
From the framework repo (`bmad_ed`):

    python3 scripts/download-mobility-datasets/download.py --only bike-sharing

The full snapshot lands under
`courses/mobillity-univ/content/datasets/bike-sharing/barcelona-bicing/`.
Copy `station-snapshots-2019-03.csv` into this `data/barcelona-bicing/` folder if you
want to run examples against the full dataset.

See `barcelona-bicing/dataset-card.md` for columns, quirks, and the honest caveat that
no public *trip-level* Bicing data exists — this is per-station state.
