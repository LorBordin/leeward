# Overview

## Question answered

> What changed at altitude in the last 24 / 48 / 72 h and over the 7 days before the outing,
> near this objective — and how far can I trust the data?

## v0 scope

| Item | v0 |
|---|---|
| Area | South Tyrol only — Provincia di Bolzano station network |
| Objectives | north faces / steep snow-and-ice terrain (e.g. Cima Vertana N, Hochferner N) |
| Data | observations only (no forecasts) |
| Output | static HTML dashboard, reachable on the home LAN |
| Runtime | Raspberry Pi, hourly systemd timer |
| History | rolling, starting 7 days before the first run (backfill) |

## Non-goals (v0)

- Forecasting.
- Computing an avalanche danger level. leeward shows indicators, not verdicts.
- Ice-fall formation model (see `roadmap.md`).
- Trentino and Tirol providers (v1).
- Mobile layout / remote access (v1, via Tailscale).

## Guiding principle: data quality is an output

1. No value without a status.
2. Never fill gaps silently.
3. Worst status wins.
4. The system itself has a status (stale collector ⇒ loud banner).
5. Representativeness is explicit: station ≠ face. Distance, elevation difference and aspect
   are always shown next to the data.

## Disclaimer (shown on every dashboard page)

leeward summarises automatic station data, which can be wrong, missing or unrepresentative of
the terrain you will be on. It does not replace the avalanche bulletin or on-site assessment.
