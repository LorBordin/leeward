# v0-02 — Scaffold, config, DB

## Do

- `uv` project, package `leeward`, `typer` CLI with stub commands.
- `config.py`: pydantic models for `leeward.toml` and `targets.yaml`; `leeward.example.toml`
  with every threshold from `qc.md` and `targets/north-face.md`.
- Loader rejects `verified: false` targets unless `--allow-unverified`.
- `db.py`: migrations from `01-architecture.md`, upsert helpers.

## Tests

- Config validation: missing fields, unverified targets, bad units.
- Migrations idempotent; upsert on duplicate key keeps the newest `fetched_at`.

## DoD

`uv run pytest` green; `uv run leeward --help` lists all commands.
