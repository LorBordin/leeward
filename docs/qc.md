# Data quality

## Statuses

Ordered best → worst:

| Status | Meaning |
|---|---|
| `OK` | coverage ≥ threshold, no flags |
| `DEGRADED` | partial coverage, stale, or a check could not run — usable with caution |
| `SUSPECT` | data present but a plausibility check failed |
| `MISSING` | no data, or coverage below the minimum |

## Propagation

- A derived value's status is `max(status of every input sample/series used)`.
- `detail_json` lists *which* inputs caused the status, so the dashboard can explain it.
- A check that cannot run returns `DEGRADED` with reason `check_not_run:<check_id>`, never `OK`.

## Checks

Initial thresholds, **to be tuned on real data**. All live in `leeward.toml` under `[qc]`.

| id | Rule (initial) | Result |
|---|---|---|
| `coverage` | received / expected samples in window. < 0.90 → `DEGRADED`, < 0.50 → `MISSING`. Expected count derives from the sensor's measured step (see `data-sources/bz.md`), not an assumed one. | per window |
| `staleness` | last sample age > 1 h → `DEGRADED`, > 6 h → `MISSING` | per sensor |
| `range` | TA ∉ [-45, 35], RH ∉ [0, 105], WS ∉ [0, 70], HS ∉ [0, 800], WD ∉ [0, 360] | `SUSPECT` (sample) |
| `flatline` | identical value ≥ 6 h for WS, TA, RH | `SUSPECT` |
| `rimed_anemometer` | WS = 0 for ≥ 3 h while RH > 95 % and TA < 0 | `SUSPECT` + note "possible rimed anemometer" |
| `hs_spike` | |ΔHS| > 20 cm in one step that reverts within 1 h | `SUSPECT` (sample) |
| `hs_noise` | ΔHS is significant only if > max(3 cm, 3σ of HS noise over the previous 24 h without precipitation). If σ cannot be estimated → `DEGRADED`. | annotation used by metrics |
| `hs_precip_consistency` | ΔHS > +10 cm with precip ≈ 0 → wind deposition or sensor error; precip > 10 mm with ΔHS ≈ 0 and TA < 0 → wind erosion or sensor error | `SUSPECT` + note |
| `gauge_undercatch` | TA < 1 °C and precip > 0 | fixed note: gauges under-catch snow |
| `source` | HTTP error, timeout, empty response, schema mismatch | `fetch_log`; provider health |

## Provider health

Per provider: last successful fetch, error rate over the last 24 h, stations with no data in the
last 6 h. Shown on the dashboard header. Any provider without a success in 2 h → red banner.

## What QC never does

- Modify, drop or replace raw samples.
- Turn `SUSPECT` into `OK` because another station "looks fine".
