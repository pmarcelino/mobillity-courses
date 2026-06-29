# Mobillity University — Course Materials

Hands-on materials for **Mobillity University**, a course on doing real data
analysis with the help of an LLM, using public European mobility data
(bike-share, rail GTFS, national origin-destination flows, Eurostat KPIs).

Each module folder is self-contained: a short README, the learner notebook(s),
the datasets they need (or a `GET-DATA.md` when a dataset is too large to ship),
and the module's HTML "bumper" videos as static pages.

## Modules

| Module | Topic | Notebooks | Data |
|--------|-------|-----------|------|
| 1 | AI fluency | — (conceptual) | — |
| 2 | Getting started | — | Barcelona Bicing (sample) |
| 3 | Cleaning & exploring | station-snapshots, system-usage, trips | Bicing synthetic set |
| 4 | Visualizing | the iteration loop (worked example) | MITMA O-D (via GET-DATA) |
| 5 | Evaluating | auditing an analysis; where to add service | Catalonia FGC GTFS; MITMA |
| 6 | Telling the story | structuring a data story; Colab → deck | Catalonia FGC GTFS; Eurostat |

## How to use

- **Google Colab (no install):** open any notebook and click its **Open in Colab**
  badge. The first cell clones this repo and locates the data automatically;
  large datasets download on first run.
- **Locally:** clone the repo, `cd` into a module folder, and open the notebook
  from there (so the notebook's `./data` resolves). Install the libraries the
  notebook imports — typically `pip install pandas plotly matplotlib seaborn geopandas`.

## Data policy

Datasets under ~100 MB are bundled directly. Anything larger ships as a
`GET-DATA.md` in that module's `data/` folder with the exact source URL and a
one-line download command; the notebooks' setup cell fetches it on first run in
Colab. Every dataset folder carries a `dataset-card.md` documenting its source,
schema, and upstream license.

## License

Course content (notebooks, READMEs, bumpers) is licensed **CC BY 4.0** — see
`../LICENSE`. Bundled datasets remain under their own upstream licenses, recorded
in each `dataset-card.md` (Open Data BCN, MITMA, FGC, Eurostat).
