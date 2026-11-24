# AQDI Methodology — How we make sensor data decision-grade

Open documentation of the quality-control and analysis methodology behind FengHeng Analytics' Air Quality Data Infrastructure (AQDI). Published because **public environmental data deserves public methods** — city partners and citizens should be able to audit how raw sensor readings become published numbers.

- [The AQDI pipeline](aqdi-pipeline.md)
- [QC framework: fault classes and screening rules](qc-framework.md)
- [AQI computation (US EPA 2024 breakpoints)](aqi-epa-2024.md)

## Principles

1. **Raw and calibrated values are stored side-by-side.** Vendor calibration is a claim, not a truth. `raw` (sensor output) and `value` (vendor-calibrated) are both persisted; every published series states which one it uses.
2. **A "calibrated-ready" status is not the final word.** Vendor QC passing only means the payload arrived intact. Physical plausibility is checked separately by our QC layer.
3. **Quarantine, never delete.** Suspect readings are flagged and excluded from aggregates, but retained in the record for audit and reanalysis.
4. **Failures are documented per channel.** A station is a bundle of independent sensors (PM1, PM2.5, PM10, NO2, O3, T, RH); one dead channel doesn't kill a station, and one healthy channel doesn't vouch for its neighbors.

## Operational context

The methodology below is exercised daily on a 12-station network in Freetown, Sierra Leone, run in partnership with Freetown City Council. Network fully deployed June 2026; ~1.5M hourly measurements and growing.

## License

CC BY 4.0 — use it, cite it, hold us to it.
