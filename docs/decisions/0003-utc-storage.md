# 0003 — Store all times in UTC

**Status:** accepted

Providers use local or CET/CEST-suffixed timestamps. Mixing them across providers and DST
transitions is a classic source of silent shifts of one hour, which would misalign wind and
snow events. All timestamps are converted to UTC at ingestion and rendered in Europe/Rome.
Parsers are tested on both DST transition days.
