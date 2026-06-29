---
dataset_id: bicing-barcelona-synthetic-system-usage
synthetic: true
generated_by: courses/mobillity-univ/docs/notebooks/notebook-3.3-system-usage.ipynb
source_url: synthetic — fabricated to match the schema of bicing-barcelona-2019-03 system-usage
download_date: 2026-06-28
license: CC0 — synthetic data, contains no real records
update_cadence: static — regenerate by re-running the notebook (seed=42)
format: CSV (comma-delimited, UTF-8)
time_coverage: synthetic 2019-03-01 → 2019-03-18 (5-minute cadence)
---

# Synthetic Bicing system-wide usage — `system-usage-sample.csv`

**Synthetic, fake-but-plausible** system-wide usage time series (5,184 rows, one row per
5 minutes). It reproduces the schema of the real `system-usage-2019-03.csv` plus the
outlier patterns story-3.3 teaches. **Not real data.**

Used by **story-3.3** (detecting and handling outliers).

## Schema

| Column | Type | Notes |
|--------|------|-------|
| `error` | int | Sensor error flag: 0 normal, 1 during the planted outage window. |
| `bikesInUsage` | int | System-wide bikes in use. Right-skewed; contains planted anomalies. |
| `electricalBikesInUsage` | int | E-bikes in use (~12% of `bikesInUsage`). |
| `mechanicalBikesInUsage` | int | Mechanical bikes in use (`bikesInUsage` − electrical). |
| `dateTime` | string | ISO `YYYY-MM-DD HH:MM:SS`, 5-minute cadence over March 2019. |

## Sample rows

```
error,bikesInUsage,electricalBikesInUsage,mechanicalBikesInUsage,dateTime
0,34,4,30,2019-03-01 00:00:00
0,18,2,16,2019-03-01 00:05:00
0,39,5,34,2019-03-01 00:10:00
1,0,0,0,2019-03-05 15:00:00
0,1500,180,1320,2019-03-14 09:00:00
0,658,79,579,2019-03-16 12:00:00
```

(Row 4 is the sensor outage; row 5 is a physically impossible value; row 6 is a genuine
event surge.)

## Known quirks — the pedagogical gold

Seed-stable:

- **Right-skewed distribution** (skew ≈ 0.75) — usage is low at night and high across a
  long daytime body. The IQR fence sits **above** routine rush peaks, so IQR flags only
  genuine anomalies; a mean-based Z-score is the wrong lens. → **story-3.3** (pick the
  method that matches the shape).
- **Sensor outage** — `bikesInUsage == 0` with `error == 1` at **2019-03-05 (a Tuesday)
  15:00–15:15**. Zero at 3 pm is suspicious (typical 15:00 ≈ 172), while near-zero at
  03:00 (≈ 25) is normal night behaviour. → **story-3.3** (event-vs-error via timestamp
  context; the 0 sits inside the night range, so IQR does *not* flag it — context does).
- **Genuine event surge** — ~625–660 bikes at **2019-03-16 12:00–12:10**. IQR-flagged;
  this is real signal, so keep-and-flag. → **story-3.3**.
- **Physically impossible values** — `-4` at 2019-03-10 04:00 (negative) and `1500` at
  2019-03-14 09:00 (above the ~900 fleet ceiling). Caught by **domain thresholds**; the
  negative is below the IQR lower fence's reach, so domain logic catches what IQR misses.
  → **story-3.3**.
- IQR detection flags exactly 4 rows (the event surge + the impossible `1500`); capping at
  the upper fence (≈ 481) pulls skew from ≈0.76 down to ≈0.15.

## Regenerate

Open `courses/mobillity-univ/docs/notebooks/notebook-3.3-system-usage.ipynb`
and run **Section 0**. Seeded (`np.random.default_rng(42)`) → reproducible byte-for-byte.
