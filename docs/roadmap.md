# Roadmap

## v1 — more providers, remote access

- [ ] Trentino provider (Meteotrentino): find open data endpoints, licence, sensors.
- [ ] Tirol provider (LWD Tirol / GeoSphere Austria): find public endpoints and terms of use.
- [ ] Cross-provider consistency checks.
- [ ] Tailscale access + mobile layout.
- [ ] Evening summary notification (Thu/Fri) with statuses.

## Ice falls — separate model, after v0

- [ ] Literature review of ice-fall formation models (freezing degree-days, energy balance,
      water supply) **before** writing code.
- [ ] Method: for every outing, collect the 7 days of data before it and record on-site
      observations (date, ice fall, observed state: formed / thin / absent / thawing).
      This becomes the calibration dataset.
- [ ] Candidate inputs: freezing degree-days at ice-fall elevation, direct-sun hours from sun
      position + DEM horizon (South Tyrol / Tirol LiDAR DTMs), thaw events, precipitation in the
      preceding weeks, avalanche exposure from slopes above.
- [ ] Output as a heuristic index, not a probability, until calibrated.
- [ ] New target type `ice_fall` reusing the same QC and status machinery.

## Later

- [ ] INCA / SNOWGRID gridded data (GeoSphere) to estimate conditions at the target point.
- [ ] Euregio avalanche bulletin as context (not as an input to any metric).
