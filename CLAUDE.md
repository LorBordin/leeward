# CLAUDE.md — rules for coding agents

This project feeds decisions about exposure to avalanche terrain. A silent data error can get
someone hurt. The rules below are not style preferences; treat them as requirements.

## Non-negotiable rules

1. **Never fill gaps silently.** No `0` for missing, no forward-fill, no interpolation unless the
   result is explicitly labelled as interpolated and carries a degraded status.
2. **Every derived value carries a status and a coverage.** Statuses: `OK`, `DEGRADED`,
   `SUSPECT`, `MISSING` (see `docs/qc.md`). A derived value takes the **worst** status of its inputs.
3. **QC flags annotate, they never modify raw data.** `raw_obs` is append/upsert only and stores
   exactly what the provider returned (plus `fetched_at`).
4. **Unknown is not OK.** If a check cannot run (e.g. not enough data to estimate sensor noise),
   the result is `DEGRADED` with a reason, never `OK`.
5. **Store time in UTC.** Convert at ingestion, render in Europe/Rome. Provider timestamp formats
   must be parsed explicitly and tested, including DST transitions.
6. **Units are explicit.** Canonical internal units are listed in `docs/01-architecture.md`.
   Convert at ingestion; never mix units in the DB.
7. **No thresholds in code.** All QC and metric thresholds live in `leeward.toml`.
8. **Verify, don't assume, provider behaviour.** Anything about a provider that is not confirmed
   by `leeward check-api` output is marked *unverified* in `docs/data-sources/`.
9. **The dashboard must show its own age** and the age of the last successful fetch per provider.

## Workflow

- Work through `docs/tasks/` in order. Each task has a definition of done; do not mark a task
  done until its tests pass.
- If reality contradicts a doc (e.g. the API behaves differently), update the doc in the same
  change and note it in the task file.
- Design changes that affect the rules above need a new ADR in `docs/decisions/`.
- Tests: `pytest`. QC logic must be tested with synthetic series containing gaps, spikes,
  flatlines, DST transitions and out-of-range values.
- Language: code, comments, docs and commit messages in English.

## Stack

Python ≥ 3.11, `httpx`, `sqlite3` (stdlib), `pydantic` v2, `jinja2`, `typer` for the CLI,
`pytest`. Dependency management with `uv`. Keep dependencies minimal: this runs on a Raspberry Pi.
