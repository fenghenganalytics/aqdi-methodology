# AQI computation — US EPA 2024 breakpoints

We convert QC-screened 24-hour PM2.5 means to the US EPA AQI using the 2024 revision of the breakpoints (annual NAAQS revision, PM2.5 scale updated accordingly). Computed at load time from the published series — **AQI is never stored as an authoritative value**, it is always derivable from the underlying mean.

## Reference

- US EPA, 2024 revisions to the Air Quality Index and annual PM2.5 standard: <https://www.epa.gov>
- Breakpoint table (µg/m³ 24-h PM2.5 → AQI) implemented in the dashboard front-end

## Rounding & disclosure rules

- PM2.5 means rounded to 1 decimal before AQI lookup
- Invalid/under-review channels show maintenance states — the AQI is withheld for that station, not substituted from neighbors
