# v0-03 — bz provider and collector

## Do

- `providers/base.py` per `data-sources/_provider-interface.md`.
- `providers/bz.py`: mapping, unit handling, timestamp → UTC, using facts from v0-01.
- `collect`: window = [last_success − overlap, now], extended to cover gaps (capped);
  one failing series never aborts the run; every call → `fetch_log`.
- `backfill --days N [--target ID]`.
- Polite rate: configurable delay between requests.

## Tests

- Parser on recorded fixtures, including the DST transition fixture.
- Nulls/sentinels preserved as `value = NULL` with `raw_value` kept.
- Simulated HTTP errors/timeouts → logged, run continues, exit code non-zero only if a provider
  had zero successes.
- Gap healing: last success 20 h ago → window extended.

## DoD

Running `collect` twice in a row on live data yields no duplicates and a clean `fetch_log`.
