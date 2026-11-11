# Deployment (Raspberry Pi)

Development happens on the MiniPC; the Pi only runs the tool.

## Setup

```bash
git clone git@github.com:lorbordin/leeward.git ~/leeward
cd ~/leeward
uv sync
cp leeward.example.toml leeward.toml
cp docs/targets/targets.example.yaml targets.yaml   # then fill and verify
uv run leeward check-api           # first command on any new machine
uv run leeward backfill --days 7
```

## systemd

`leeward-run.service` (oneshot, runs `leeward run`) and `leeward-run.timer`
(`OnCalendar=hourly`, `Persistent=true`). Unit files live in `deploy/systemd/`.

Dashboard: `leeward-web.service` serving `dashboard/` on the LAN.

## Logs and backups

- Logs to journald (`journalctl -u leeward-run`).
- Daily SQLite backup with `sqlite3 .backup`, keep 14 copies.

## Health

`leeward run` exits non-zero when a provider had no successful fetch, so failures are visible
with `systemctl list-units --failed`.
