# Synthetic MITMA-style province flows for the Module 4 choropleth stretch

This folder holds one **small, synthetic, fake-but-plausible** Spain
district-to-district flow table built specifically for the choropleth "going further"
task in the Module 4 geographic-visualization exercise
(`docs/exercises/exercise-4.5-geographic-visualization-with-plotly.md`).

It is **not real data.** The real MITMA daily O-D file is ~203 MB compressed and
pipe-delimited — far too heavy for a Colab exercise — so this 347-row seeded stand-in
reproduces the column semantics that matter for the join: `origen`/`destino` district
codes whose **first two digits are the INE province code**, and float `viajes`
estimates. Madrid (28) is the planted dominant origin, Barcelona (08) second, and all
52 provinces appear so the whole map shades.

| CSV | Rows | Card |
|-----|------|------|
| `province_flows_practice.csv` | 347 | [province-flows-card.md](./province-flows-card.md) |

The choropleth's boundary file is **not stored here** (and must never be committed to
the published tree): the exercise points at the public Spain-provinces GeoJSON
(`https://raw.githubusercontent.com/codeforgermany/click_that_hood/main/public/data/spain-provinces.geojson`,
join key `cod_prov`, 52 features, zero-padded 2-char codes).

## Regenerate

Run the module's seeded builder:

```
.venv/bin/python courses/mobillity-univ/docs/notebooks/build-module-4-lab.py --practice <dir>
```

It writes this CSV (seed 7) byte-for-byte here, and the four Bicing-style practice CSVs
into the sibling `barcelona-bicing-viz-synthetic/` folder. The dataset card is
reconciled against the CSV with `scripts/validate_examples/check_card.py`. These assets
were produced by the `build-module-lab` skill (exercise-only scope).

> **Relationship to the real data:** the real MITMA download lives in
> `../mitma-spain-od/` (this folder sits beside it at the datasets root). This synthetic
> stand-in mirrors its column semantics for teaching only and does not replace it.
