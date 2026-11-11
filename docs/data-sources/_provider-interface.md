# Provider interface

Every provider implements the same protocol so QC, metrics and dashboard never know where data
came from. Needed in v0 for `bz` only, but designed for Trentino and Tirol (v1).

```python
class Provider(Protocol):
    id: str                                   # "bz", "tn", "tirol", …

    def list_stations(self) -> list[Station]: ...
    def list_sensors(self, station_code: str) -> list[SensorInfo]: ...
    def fetch(
        self, station_code: str, sensor: str,  # internal sensor id
        start_utc: datetime, end_utc: datetime,
    ) -> FetchResult: ...
```

`FetchResult` contains the parsed samples (UTC, canonical units), the raw values, the HTTP
status, and any parse warnings. Parsing problems are **returned, not raised**, so the collector
can log them and continue.

Each provider module owns:

- sensor code mapping → internal ids (`data-sources/<provider>.md`)
- unit conversion to canonical units
- timestamp parsing → UTC, with tests on DST transitions
- the list of its *unverified* behaviours

## Cross-provider checks (v1)

Nearby stations from different networks that diverge beyond a threshold are flagged on both
sides. Never resolved automatically.
