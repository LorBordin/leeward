# 0001 — SQLite as the only datastore

**Status:** accepted

Single user, one writer (the hourly run), a few stations × sensors at 10-minute resolution:
tens of thousands of rows per month. SQLite needs no server, is trivial to back up and runs well
on a Raspberry Pi. Revisit only if gridded data (INCA/SNOWGRID) is added; rasters stay on disk,
not in SQLite.
