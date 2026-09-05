# NOD-SBC-001 — ED-18 Power-Rail Characterization & Supervisory MCU Bring-Up

## Status

**CONTROLLED EXECUTION PACKAGE ESTABLISHED — PHYSICAL EXECUTION NOT YET PERFORMED**

ED-18 converts the ED-17 first-energization boundary into the next controlled verification session. No measurement is represented as PASS without attributable prototype evidence.

## Objective

Characterize the populated prototype power rails after an authorized ED-17 first energization and establish the STM32H573 supervisory MCU as the controlled health/sequence observer.

## Preconditions

- ED-16 readiness review disposition = GO.
- Prototype configuration identity and BOM reconciliation complete.
- Released schematic/PCB revision identified.
- Instrument calibration current and traceable.
- Firmware/software hashes recorded.
- ED-17 first-energization records available.
- Applicable acceptance limits released or explicitly marked TBC/TBD.

## Test sequence

### ED18-PWR-01 — Pre-power electrical sanity

Record input polarity, resistance-to-ground checks, continuity, connector population, and visible inspection. Resolve discrepancies before energization.

### ED18-PWR-02 — Primary input characterization

Record source voltage, inrush behavior where instrumentation permits, input current, protection status, and steady-state input behavior. Do not exceed the released input envelope.

### ED18-PWR-03 — Rail characterization

For every released rail, record nominal setpoint, measured DC voltage, startup behavior, ripple/noise where specified, load state, and regulator thermal observation. Compare only against controlled acceptance limits.

### ED18-PWR-04 — Power-good and fault behavior

Exercise approved enable/reset conditions and observe PGOOD, fault, current-limit, and shutdown indications. Fault injection is performed only where explicitly authorized by the applicable procedure.

### ED18-MCU-01 — STM32H573 supervisory bring-up

Verify reset release, boot identity, clock/health telemetry, ADC/current/voltage monitoring interfaces, watchdog service, fault inputs, and controlled shutdown signaling.

### ED18-MCU-02 — Supervisory fault response

Apply approved non-destructive fault conditions and verify deterministic detection, logging, recovery or shutdown behavior, and evidence capture.

### ED18-PWR-05 — Controlled shutdown / restart

Verify commanded shutdown and restart behavior, retained fault state where required, and absence of uncontrolled back-powering.

## Required evidence

- Prototype serial/configuration identifier
- Board revision and BOM revision
- Firmware/software SHA-256 identifiers
- Instrument IDs and calibration status
- Input voltage/current records
- Rail voltage/current/ripple records where applicable
- Oscilloscope captures for controlled transient observations
- PGOOD/reset timing captures where applicable
- MCU console/telemetry logs
- Fault-log records
- Photographic inspection evidence where useful
- Test-record signatures and disposition

## Result states

`PASS` · `FAIL` · `BLOCKED` · `NOT-RUN` · `N/A`

A test may be PASS only when the acceptance criterion is controlled and the required objective evidence is attached to the record.

## Exit criteria

1. All executed ED18 tests have completed records.
2. Every measurement has attributable configuration and instrument identity.
3. All failures/deviations have NCR or engineering disposition.
4. No unresolved unsafe condition remains.
5. Supervisory MCU health path is demonstrated or formally dispositioned.
6. Power-rail characterization is reconciled against the controlled design limits.
7. ED-18 evidence manifest is complete.

## Next gate

**ED-19 — SoM boot, compute-domain initialization, and controlled software bring-up.**