# Architecture

## Pipeline

```
systemd timer (hourly, Persistent=true)
  └─ leeward run
       ├─ collect    for each (provider, station, sensor) in config:
       │               fetch window [last_success - overlap, now], upsert raw_obs, write fetch_log
       ├─ qc         recompute qc_flags for touched windows
       ├─ aggregate  compute metrics per target × window → target_metrics (with status, coverage)
       └─ render     write dashboard/index.html (static, includes generation time)
```

- **Idempotent collection.** Each run re-fetches an overlap (default 6 h) so missed runs heal.
  If the last success is older than the overlap, the window extends back to cover the gap,
  capped by `collect.max_window_days`.
- **Backfill.** `leeward backfill --days 7 [--target ID]` fetches history for new targets.
- **Failure isolation.** One failing station/sensor never aborts the run; it is logged in
  `fetch_log` and surfaces as a status on the dashboard.
- **Exit codes.** `leeward run` exits non-zero if *any* provider had zero successful fetches,
  so systemd/journald show it.

## Canonical internal units

| Quantity | Unit |
|---|---|
| air temperature | °C |
| relative humidity | % |
| precipitation | mm (per sample interval) |
| wind speed, gust | m/s (dashboard also shows km/h) |
| wind direction | degrees, 0–360, meteorological (direction *from*) |
| snow depth | cm |
| global radiation | W/m² |
| sunshine duration | s (per sample interval) |
| time | UTC, ISO 8601 |

## Data model (SQLite)

```sql
CREATE TABLE stations (
  provider     TEXT NOT NULL,
  code         TEXT NOT NULL,
  name         TEXT,
  lat          REAL, lon REAL, elevation_m REAL,
  meta_json    TEXT,
  updated_at   TEXT NOT NULL,
  PRIMARY KEY (provider, code)
);

CREATE TABLE raw_obs (
  provider     TEXT NOT NULL,
  station_code TEXT NOT NULL,
  sensor       TEXT NOT NULL,      -- internal sensor id (see data-sources mapping)
  ts_utc       TEXT NOT NULL,
  value        REAL,               -- NULL allowed: provider sent a null/sentinel
  raw_value    TEXT,               -- exactly what the provider sent
  fetched_at   TEXT NOT NULL,
  PRIMARY KEY (provider, station_code, sensor, ts_utc)
);

CREATE TABLE fetch_log (
  id           INTEGER PRIMARY KEY,
  run_id       TEXT NOT NULL,
  provider     TEXT NOT NULL,
  station_code TEXT, sensor TEXT,
  window_from  TEXT, window_to TEXT,
  started_at   TEXT NOT NULL,
  duration_ms  INTEGER,
  http_status  INTEGER,
  rows         INTEGER,
  error        TEXT                -- NULL on success
);

CREATE TABLE qc_flags (
  provider     TEXT NOT NULL,
  station_code TEXT NOT NULL,
  sensor       TEXT NOT NULL,
  ts_from      TEXT NOT NULL,
  ts_to        TEXT NOT NULL,
  check_id     TEXT NOT NULL,
  severity     TEXT NOT NULL,      -- DEGRADED | SUSPECT | MISSING
  detail       TEXT,
  computed_at  TEXT NOT NULL
);

CREATE TABLE target_metrics (
  run_id       TEXT NOT NULL,
  target_id    TEXT NOT NULL,
  window       TEXT NOT NULL,      -- 24h | 48h | 72h | 7d
  metric       TEXT NOT NULL,
  value        REAL,
  status       TEXT NOT NULL,
  coverage     REAL,               -- 0..1
  detail_json  TEXT,               -- reasons, inputs used, flags inherited
  computed_at  TEXT NOT NULL,
  PRIMARY KEY (run_id, target_id, window, metric)
);
```

Schema migrations: numbered SQL files in `leeward/migrations/`, applied at startup.

## Package layout (proposed)

```
leeward/
  cli.py             typer app: run, collect, qc, aggregate, render, backfill, check-api
  config.py          pydantic models for leeward.toml and targets.yaml
  db.py              connection, migrations, upserts
  providers/
    base.py          Provider protocol (see data-sources/_provider-interface.md)
    bz.py            Provincia di Bolzano
  qc/                one module per check + status algebra
  metrics/           north_face.py
  render/            jinja2 templates, charts
tests/
  fixtures/          recorded provider responses (from check-api), synthetic series
```

## Configuration

- `leeward.toml`: paths, overlap, thresholds (QC + metrics), provider settings.
- `targets.yaml`: objectives (see `targets/`).
- Config is validated at startup; invalid or unverified targets fail loudly.
