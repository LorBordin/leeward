# v0-06 — Dashboard

## Do

Implement `dashboard.md` with jinja2 and inline SVG charts.

## Tests

- Snapshot test of rendered HTML from a fixture DB.
- `MISSING` metric renders "no data", never a number.
- Stale generation / stale provider → banner present.
- Gaps rendered as gaps (no line across missing samples).

## DoD

Dashboard opened on the LAN shows both targets with live data and is understandable without
reading the docs.
