# v0-01 — check-api

Goal: replace assumptions about the Bolzano API with facts, before any production code.

## Do

1. Small script (later `leeward check-api`) that:
   - fetches `stations`, prints count and the high stations (> 2000 m) with code, name, elevation;
   - for each candidate station in `data-sources/bz.md`, fetches `sensors` and lists available codes;
   - fetches 3 days of `timeseries` for `LT`, `HS`, `WG`, `WR`, `N` at `06090SF`;
   - fetches a range spanning the last DST transition (2026-10-25) for one sensor;
   - reports: timestamp format and suffixes seen, sampling step (histogram of deltas), gaps,
     nulls/sentinels, whether `N` looks per-step or cumulative, response times, HTTPS support.
2. Save raw responses to `tests/fixtures/bz/` (they become parser test fixtures).
3. Identify Hochferner stations (Wilder Freiger signal summit code; highest Val di Vizze station).

## DoD

- Every item in `bz.md` → "Unverified" is ticked or rewritten with the observed behaviour.
- Candidate station table is complete.
- Fixtures committed.
