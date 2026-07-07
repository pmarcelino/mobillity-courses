# FGC schedule (GTFS) — frozen snapshot

The scheduled public-transport service published by **Ferrocarrils de la
Generalitat de Catalunya (FGC)** for the Barcelona / Catalonia rail network, in
the standard **GTFS** format (the same feed transit apps consume).

- **Source:** <https://www.fgc.cat/google/google_transit.zip> (page: <https://www.fgc.cat/en/opendata/>)
- **License:** Open data (FGC OpenData) — see the source page.
- **Snapshot:** downloaded **2026-04-21**, published here **unmodified** so the
  Module 5 worked example reproduces exactly. FGC refreshes its live feed
  (typically weekly), so this frozen copy — not the live URL — is what the
  notebook loads.

## Files here

| File | Rows | What each row is |
|------|------|------------------|
| `routes.txt` | 21 | one line, with a `route_type` mode code |
| `trips.txt` | 17,294 | one scheduled trip, linked to a line and a service calendar |
| `stop_times.txt` | 190,182 | one stop of one trip, with a scheduled `departure_time` |
| `calendar_dates.txt` | 1,316 | which `service_id` runs on which date (`exception_type` 1 = runs) |
| `agency.txt`, `feed_info.txt` | — | publisher / feed provenance |

(The full feed also ships `stops.txt`, `shapes.txt`; they aren't needed here.)

## Quirks the worked example relies on

- **`route_type` is a coded integer**, and this feed mixes only `1` = metro
  (4 lines), `2` = rail (14 lines), `7` = funicular (3 lines). There are **no
  buses** (code `3`). GTFS's full list is broader (0=tram, 3=bus, 4=ferry, …).
- **`departure_time` can exceed `24:00:00`** — GTFS writes an after-midnight
  departure of a pre-midnight service as e.g. `24:15:00` or `25:30:00`. A naïve
  `pd.to_datetime(..., format="%H:%M:%S")` crashes on the first such value;
  read the time as *minutes since midnight* instead.
- A representative weekday in this snapshot is **`20260414`** (a Tuesday):
  ~2,041 trips across 20 lines.

## Loading it

```python
import pandas as pd
BASE = ("https://raw.githubusercontent.com/pmarcelino/mobillity-courses/"
        "main/mobillity-univ/module-5-evaluating/data/gtfs-catalonia-fgc")
routes = pd.read_csv(f"{BASE}/routes.txt", dtype=str)
```
