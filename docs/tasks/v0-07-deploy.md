# v0-07 — Deploy on the Pi

## Do

- `deploy/systemd/` unit files per `deploy.md`.
- Backup script.
- Run `check-api` on the Pi, then `backfill --days 7`, then enable the timer.

## DoD

- Timer runs hourly for 48 h without failures (or failures are visible and explained).
- Pull the network cable for an hour: the dashboard shows the stale-data banner, and data heals
  on the next run.
