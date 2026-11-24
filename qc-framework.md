# QC framework — fault classes and screening rules

Field sensor networks fail in known, repeatable ways. These are the fault classes we detect and how we screen them.

## 1. Optical saturation fingerprint

Low-cost optical PM sensors pin when the chamber is overloaded.

**Signature:** PM1 ≈ PM2.5 ≈ PM10 (all three size bins within 1 µg/m³ of each other) AND raw > 500 µg/m³.
**Interpretation:** chamber pinned — hardware artefact, **not** pollution.
**Action:** quarantine reading; flag channel for review.

## 2. Station flatline

Electrochemical/optical channels can flatline (stuck ADC, failed pump, power brownout).

**Signature:** near-zero variance across all channels for a sustained window; e.g. PM2.5 hugging 0.6–1.0 µg/m³ with PM10 = 0 and NO2 absent.
**Action:** exclude station from aggregates; keep record for hardware forensics; alert ops.

## 3. Calibration inversion

Vendor calibration coefficients occasionally invert the physical ordering.

**Signature:** PM2.5 > 1.5 × PM10 sustained over hours (physically implausible — fine mass cannot exceed the coarse bin that contains it).
**Action:** exclude the affected window from exceedance statistics; flag for vendor recalibration.

## 4. Electrochemical zero-drift

NO2/O3 electrochemical cells drift negative with age and temperature.

**Signature:** readings within −30 ppb of zero.
**Action:** keep in record; exclude from exceedance counts (small negatives are instrument noise, not negative air).

## 5. Out-of-range quarantine

Raw PM outside 0–500 µg/m³ is physically suspect on this hardware class.
**Action:** quarantine pending review.

## 6. Channel-level suppression

Individual channels die while station survives (e.g. O3 cell failure after humidity ingress).
**Action:** station stays live on remaining channels; dead channel marked suppressed in the public dashboard rather than silently dropped — citizens see what's not being measured.

## Derived QC views

The production system materializes these as ready-made views: QC-passed subset, per-channel flags, zero-event census, gap census, monotonicity violations, implausible-O3 ledger. Aggregates are **always computed from the QC-passed subset**; raw feeds are never mixed into published numbers.
