# The AQDI pipeline

```
 Clarity sensor API ─┐
 CAMS aerosol/dust   ├─→ ingestion (hourly/idempotent) ─→ QC layer ─→ warehouse ─→ analytics ─→ public dashboard
 ERA5 meteorology    │                                     │
 FIRMS/GFAS fire     ┘                                     └─→ ingestion_log / channel_health (audit)
```

## Ingestion

- Idempotent window pulls (safe to re-run; revisions upsert, never duplicate)
- Every pull writes an `ingestion_log` row (window, stamps, rows new/revised, stations reporting, ok/error) — the pipeline journals its own health
- UTC hour-boundary timestamps; site-local = UTC+0 for Freetown

## Warehousing

Relational Postgres core: `measurements` (the record), `stations` registry with coordinates, `metrics_dict`, ingestion/health/lineage bookkeeping, plus reference layers (satellite PM, meteorology, fire emissions, population, regional comparison cities, household energy survey aggregates).

Ready-made analysis views: latest reading per station×metric, city hourly aggregates, QC-screened city PM2.5 series, hour-of-day climatology, station-vs-city deviations, regional daily comparison.

## Analysis & forecasting

- Satellite (CAMS) vs ground cross-bias tracking
- Forecast ledgers with quantile calibration (CQR) — model outputs are versioned and read as *model outputs*, never silently blended into the measurement record
- Harmattan discrimination: is a PM episode regional dust transport or local combustion? Satellite + wind + fire-emissions layers answer this

## Publication

- Hourly export engine → public dashboard feed
- Suppressed channels are disclosed on the dashboard (maintenance/review cards), never hidden
- The dashboard derives AQI client-side from the published series — no server-side magic numbers
