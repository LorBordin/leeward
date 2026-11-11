# Provider: Provincia di Bolzano (`bz`)

Open Data Meteo V1, Autonomous Province of Bolzano. Licence: CC0.

Base URL: `http://daten.buergernetz.bz.it/services/meteo/v1/`

| Endpoint | Purpose |
|---|---|
| `stations[?output_format=CSV][&coord_sys=EPSG:3857]` | station registry (GeoJSON by default) |
| `sensors?station_code=XXX[&sensor_code=YY]` | sensors at a station + last value |
| `timeseries?station_code=XXX&sensor_code=YY&date_from=YYYYMMDD[HHMM]&date_to=…` | time series, JSON or CSV |

Documented JSON time series shape: `[{"DATE": "2016-02-29T13:10:00CET", "VALUE": 29.5}, …]`.

Docs: <https://data.civis.bz.it/dataset/misure-meteo-e-idrografiche>

## Sensor mapping

| Provider code | Internal id | Quantity | Provider unit | Conversion |
|---|---|---|---|---|
| `LT` | `TA` | air temperature | °C | — |
| `LF` | `RH` | relative humidity | % | — |
| `N` | `PR` | precipitation | mm | — |
| `WG` | `WS` | mean wind speed | **m/s** | — |
| `WG.BOE` | `WG` | gust | **m/s** | — |
| `WR` | `WD` | mean wind direction | ° | — |
| `HS` | `HS` | snow depth | cm | — |
| `GS` | `GR` | global radiation | W/m² | — |
| `SD` | `SD` | sunshine duration | s | — |

## Unverified — to be confirmed by `leeward check-api` (task v0-01)

- [ ] Timestamp suffix during DST (`CEST`?) and whether times are local or fixed CET.
- [ ] Sampling step per sensor (10 min expected; also whether `N`/`SD` are per-step or cumulative).
- [ ] How gaps appear: missing rows, `null`, or sentinel values.
- [ ] Maximum range accepted by `timeseries` per request.
- [ ] Rate limits / polite request spacing.
- [ ] HTTPS availability.
- [ ] Which high stations actually have `HS` and `WR`.
- [ ] Whether the `sensors` endpoint "last value" lags the `timeseries` endpoint.

## Candidate stations (v0)

| Target | Station | Code | Elevation | Role |
|---|---|---|---|---|
| Vertana N | Solda / Sulden Madritsch | `06090SF` | 2825 m | high |
| Vertana N | Solda / Sulden (valley) | `06400MS` | 1902 m | valley |
| Hochferner N | Signalgipfel Wilder Freiger | TBD (from `stations`) | 3399 m | high (wind) |
| Hochferner N | highest Val di Vizze / Pfitsch station | TBD | TBD | high / valley |
