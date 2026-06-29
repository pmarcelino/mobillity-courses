# Module 3 — Cleaning and Exploring Data

Hands-on companions to the Module 3 lectures: handling missing values, fixing data
types, exploring system-usage patterns, and working with trip data. Each notebook is a short, self-contained Colab walkthrough.

## Notebooks
- `notebook-3.1-handling-missing-values.ipynb` — counting and handling the different shapes of missingness.
- `notebook-3.2-fixing-data-types.ipynb` — coercing string-stored numbers, dates, and mixed types.
- `notebook-3.3-system-usage.ipynb` — exploring per-station system-usage patterns.
- `notebook-3.5-trips.ipynb` — working with trip records.

Each notebook **generates its own small, seeded synthetic dataset** in its first section (`np.random.default_rng(42)`), so every cell runs offline with no downloads — in Colab or locally. Open any notebook and Run All.

## Run it
- **Colab:** click the "Open in Colab" badge at the top of a notebook.
- **Locally:** open the notebook from this folder and Run All. The data is created in the notebook's working directory by its first section.

## data/
- `barcelona-bicing-synthetic/` — the dataset **cards** documenting the schema, columns,
  and quirks the notebooks reproduce, plus reference copies of the sample CSVs.
- `GET-DATA.md` — optional: how to fetch the real ~353 MB Barcelona Bicing snapshot.

## bumpers/
`epic-3-intro.html`, `epic-3-conclusion.html` — the module intro and conclusion pages.
