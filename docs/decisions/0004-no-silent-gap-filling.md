# 0004 — No silent gap filling; worst status wins

**Status:** accepted

The tool supports decisions about avalanche exposure. A plausible-looking number computed from
partial or broken data is more dangerous than an explicit "no data". Therefore raw data is never
modified, gaps are never filled without a label, every derived value carries a status and
coverage, and derived values inherit the worst status of their inputs.
