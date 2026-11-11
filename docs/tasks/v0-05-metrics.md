# v0-05 — North-face metrics

## Do

Implement every metric in `targets/north-face.md` for windows 24h/48h/72h/7d, each with
status, coverage and `detail_json`.

## Tests

- Circular mean of wind direction across 350°/10° (must give ~0°, not 180°).
- Lee loading: transport from S (180°) on an N-facing target → indicator on; from N → off.
- Lapse-rate estimate falls back to −6.5 K/km when valley is not `OK`, and is labelled.
- A single `MISSING` input makes the metric `MISSING` or `DEGRADED` as specified, never `OK`.
- `hs_delta` below the noise threshold is reported as not significant.

## DoD

`aggregate` on live data writes one row per target × window × metric.
