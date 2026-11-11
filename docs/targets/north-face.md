# Target type: `north_face`

Steep snow/ice terrain where the main concern before an outing is recent snow, wind loading and
warming.

## Inputs

Stations per target, each with a role:

- `high`: snow depth, wind, temperature near target elevation
- `valley`: precipitation and temperature, to estimate the lapse rate

## Windows

`24h`, `48h`, `72h`, `7d`, all ending at the latest run time.

## Metrics

| id | Computation | Notes |
|---|---|---|
| `hs_delta` | HS(end) − HS(start), on a 1 h rolling median | reported as significant only if above the `hs_noise` threshold |
| `hs_now`, `hs_max` | from `high` | |
| `precip_sum` | Σ PR | always carries the gauge-undercatch note when cold |
| `transport_hours` | hours with WS ≥ `metrics.transport_ws` (initial 5 m/s) while new snow is available | "new snow available" = positive significant `hs_delta` within the previous 72 h *or* precip with TA < 1 °C; definition to be revisited |
| `transport_dir` | wind-speed-weighted circular mean of WD over transport hours | |
| `lee_loading` | lee sector = `transport_dir` + 180°; if target aspect within ± `metrics.lee_halfwidth` (initial 45°) → indicator **"possible lee loading"** | indicator, not verdict |
| `gust_max` | max WG | |
| `ta_min`, `ta_max` | from `high` | |
| `ta_target_est` | TA at target elevation: lapse rate from `high`/`valley` if both `OK`, else −6.5 K/km | always labelled **estimate** |
| `hours_above_zero` | hours with TA > 0 °C at `high` | warming / settlement indicator |
| `gr_sum`, `sd_sum` | if available | context |

Every metric is stored with status, coverage and `detail_json` listing inputs and inherited flags.

## Representativeness block (per station, per target)

Horizontal distance, elevation difference, and a free-text note from config
(e.g. "station on a ridge, wind over-represented"). Always rendered next to the metrics.
