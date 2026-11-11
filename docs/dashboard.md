# Dashboard (v0)

Static HTML written by `leeward render` to `dashboard/index.html`. No JavaScript required to read
the essentials; charts may use small inline SVG.

## Header (always visible)

- Generation time (Europe/Rome) and age. Generated > 2 h ago → red banner. The page embeds its
  generation time, so a stale cached page is still recognisable.
- Per provider: last successful fetch, errors in the last 24 h. No success in 2 h → red banner.
- Disclaimer (see `00-overview.md`).

## Source health panel

- Recent `fetch_log` failures, grouped.
- Stations silent for > 6 h.

## One card per target

1. Name, type, verified flag (unverified → banner).
2. Stations used: role, distance, elevation difference, note.
3. Metrics table: rows = metrics, columns = 24h / 48h / 72h / 7d. Each cell shows value, a status
   badge and coverage. `MISSING` cells show "no data", never a number.
4. 7-day charts: HS, TA (+ estimated target TA, dashed), WS/WG with WD arrows, PR.
   Gaps are drawn as gaps. `SUSPECT` periods are shaded.
5. Active QC flags, in plain English.

## Status colours

Do not rely on colour alone: every badge has a text label (`OK`, `DEGRADED`, `SUSPECT`,
`MISSING`).

## Access

v0: LAN only (`python -m http.server` or nginx on the Pi). v1: Tailscale + mobile layout.
