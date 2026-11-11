# leeward

Personal tool that collects data from automatic high-altitude weather stations, checks its
quality, and summarises recent conditions for a list of mountaineering objectives
(north faces in v0; ice falls later).

> **leeward does not replace the avalanche bulletin or on-site assessment.** It is a planning
> aid. Every value it shows carries its own data-quality status. Missing or doubtful data is
> reported, never hidden.

## Status

Planning. No code yet. Start from [`docs/00-overview.md`](docs/00-overview.md);
implementation work is split into [`docs/tasks/`](docs/tasks/).

## Layout

```
CLAUDE.md                 rules for coding agents working on this repo
docs/
  00-overview.md          scope, non-goals, disclaimer
  01-architecture.md      pipeline, data model, CLI
  qc.md                   data-quality statuses, checks, propagation
  dashboard.md            v0 dashboard spec
  deploy.md               Raspberry Pi deployment
  roadmap.md              v1 and later (Trentino, Tirol, ice falls)
  data-sources/           one file per provider + common interface
  targets/                objective types, metrics, example config
  decisions/              short ADRs
  tasks/                  ordered implementation tasks with definitions of done
```
