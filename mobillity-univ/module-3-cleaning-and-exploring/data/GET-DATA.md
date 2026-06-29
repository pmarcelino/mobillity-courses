# Optional: the real Barcelona Bicing snapshot

The Module 3 notebooks **generate their own** small, seeded synthetic data inline — fake but plausible rows that reproduce every quirk the lectures show (the coordinate drop, `streetNumber` as text with `s/n`, European `updateTime`, the Tuesday-3pm sensor zero). You do **not** need to download anything to run them.

If you want to re-run the cleaning on the **real** ~353 MB snapshot:

## Source
https://opendata-ajuntament.barcelona.cat/data/en/dataset/bicing

## Easiest way
From the framework repo (`bmad_ed`):

```bash
    python3 scripts/download-mobility-datasets/download.py --only bike-sharing
```

Then copy `station-snapshots-2019-03.csv` next to the notebook and point the notebooks at it — set the `MODULE3_DATA_DIR` environment variable (or edit the `DATA_DIR` cell) to that folder. The synthetic dataset cards (`barcelona-bicing-synthetic/*-card.md`) document the schema the notebooks mirror.
