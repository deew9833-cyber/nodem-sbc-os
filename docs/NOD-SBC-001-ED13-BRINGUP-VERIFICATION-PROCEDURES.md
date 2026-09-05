# NOD-SBC-001 — ED-13 Rev A Bring-Up & Verification Procedure Set

**Revision:** Rev A  
**Status:** Procedure baseline  
**Purpose:** Convert ED-12 verification requirements into controlled executable bring-up procedures for engineering prototype validation.

## 1. Scope

ED-13 defines the controlled sequence for first-power application, supervisory-controller validation, compute boot, thermal characterization, external-interface bring-up, storage, security-chain checks, Nodem-SBC-OS HIL, and integrated-load testing.

ED-13 is a **bring-up and design-verification procedure baseline**. It is not production qualification, regulatory certification, or environmental qualification.

## 2. Entry criteria

Do not energize a prototype until the following are satisfied:

- Controlled schematic and PCB revision identified.
- Test article serial number and configuration recorded.
- Exact fitted component population recorded.
- Power-source limits and current limits established.
- Bench supply, oscilloscope, DMM, electronic load, thermal instrumentation, and network/test equipment are calibrated or within approved calibration interval.
- ESD controls established.
- Hardware emergency power-disconnect available.
- Firmware/software baseline identified by immutable version or hash.
- Acceptance limits approved for the applicable test.

## 3. Configuration record

Record before each procedure:

| Field | Required record |
|---|---|
| PCB | Part/revision/serial |
| SoM | Exact module/configuration |
| MCU | Exact P/N/revision |
| FPGA | Installed/not installed + P/N |
| BOM | Controlled revision |
| Firmware | Version/hash |
| Nodem-SBC-OS | Version/hash |
| Input source | Voltage/current limit |
| Instruments | IDs/calibration status |
| Ambient | Temperature/humidity |

## 4. Procedure ED13-PWR — Power-on / rail bring-up

### Objective

Verify that the unpopulated/partially populated power tree can be energized safely and that rails rise in the required sequence without abnormal current or fault behavior.

### Sequence

1. Inspect PCB for assembly defects, solder bridges, polarity errors, connector damage, and foreign material.
2. Verify resistance-to-ground of primary power rails against approved pre-power limits.
3. Set current-limited bench source to the approved input condition.
4. Energize input with the board in the defined safe state.
5. Record input voltage/current.
6. Measure each primary rail in the controlled sequence.
7. Capture rail rise and reset-release waveforms.
8. Verify power-good/fault indications.
9. Remove power and verify controlled discharge where required.

### Pass criteria

- No abnormal current signature.
- No component overheating, smoke, odor, audible arcing, or physical damage.
- Every measured rail is within its approved tolerance.
- Sequencing and reset dependencies conform to the controlled design.
- Protection/fault indicators behave as specified.

**Evidence:** DMM table, oscilloscope captures, source log, photographs, deviation record.

## 5. Procedure ED13-SEQ — Reset and sequencing verification

1. Connect scope probes to designated rails, power-good signals, reset outputs, and supervisory MCU signals.
2. Apply power from the approved cold-start condition.
3. Capture the complete startup sequence.
4. Repeat for controlled shutdown.
5. Repeat for input interruption/recovery.
6. Inject only approved low-risk faults defined by the test plan.
7. Record reset cause and supervisory status.

**Pass criteria:** required ordering, timing windows, reset assertion/deassertion, and recovery behavior match the controlled requirements.

## 6. Procedure ED13-MCU — MCU supervisory verification

Verify:

- MCU boots from approved firmware.
- Clock/reset initialization succeeds.
- Power telemetry channels enumerate.
- Temperature/current/voltage telemetry is readable.
- Watchdog operates.
- Fault inputs are recognized.
- Controlled shutdown/reset command works.
- Service interface access follows security policy.

Correlate telemetry against calibrated external instruments.

**Pass criteria:** all mandatory supervisory functions operate deterministically and measured telemetry remains within the approved error allocation.

## 7. Procedure ED13-SOM — SoM boot verification

1. Confirm approved boot media/image identity.
2. Apply cold power.
3. Record boot timing and console output.
4. Verify secure/authenticated boot state.
5. Verify memory/storage/network enumeration.
6. Verify operating-system health.
7. Perform controlled reboot and power-cycle tests.
8. Attempt an approved negative boot-image test in an isolated engineering configuration.

**Pass criteria:** approved image boots reliably; invalid/unapproved image is rejected according to the security architecture; no uncontrolled reset or corruption occurs.

## 8. Procedure ED13-THM — Thermal characterization

Test at defined ambient conditions and approved operating modes.

1. Record ambient temperature.
2. Instrument SoM/thermal-spreader/chassis thermal path.
3. Establish idle baseline.
4. Apply controlled compute load.
5. Apply controlled I/O load.
6. Apply combined mission load.
7. Record temperatures, clocks, power, and throttling state until thermal equilibrium or defined time limit.
8. Repeat for the applicable T1/T2/T3 load class.
9. Compare measured results with the thermal model.

**Pass criteria:** approved component temperature limits and system thermal envelope are maintained; no uncontrolled thermal runaway; model-to-test correlation is documented.

## 9. Procedure ED13-IO — Ethernet / USB / PCIe bring-up

### Ethernet

- Verify link negotiation.
- Verify required speeds.
- Run controlled throughput and packet-integrity tests.
- Repeat under concurrent compute/storage load.

### USB

- Verify enumeration for each required port/mode.
- Run controlled sustained-transfer tests.
- Verify disconnect/reconnect behavior.

### PCIe

- Verify endpoint/root-complex enumeration as applicable.
- Record negotiated generation and lane width.
- Run controlled stress and error monitoring.
- Verify recovery behavior after controlled endpoint reset.

**Pass criteria:** required interface modes enumerate and remain stable under the approved workload without unacceptable errors or data corruption.

## 10. Procedure ED13-STOR — Storage verification

Verify:

- SPI-NOR/eMMC/NVMe enumeration as fitted.
- Approved boot path.
- Controlled read/write functionality.
- File/data integrity.
- Recovery from controlled reboot.
- Invalid-image rejection where applicable.

Use hashes/checksums for test data and record all errors.

## 11. Procedure ED13-SEC — Security-chain verification

Verify the complete trust path:

```text
Hardware Root
    ↓
Boot Authentication
    ↓
Measured Platform State
    ↓
Nodem-SBC-OS
    ↓
Security Services
    ↓
Mission Software
```

Required checks:

- Device identity is unique and retrievable only through approved interfaces.
- Protected key material cannot be read through normal software paths.
- Authenticated boot rejects modified/invalid images.
- Measurements/attestation match the approved baseline.
- Secure update accepts approved images and rejects invalid images.
- Recovery path preserves security policy.
- Debug/service access controls operate as designed.

**Pass criteria:** every negative test produces the defined deny/fail-safe state and every positive test reaches the approved state.

## 12. Procedure ED13-HIL — Nodem-SBC-OS hardware-in-the-loop

Verify:

- HAL hardware enumeration.
- Power telemetry API.
- Thermal telemetry API.
- Security-state API.
- MCU command/status path.
- Watchdog service.
- Fault reporting.
- Storage and network discovery.
- Controlled reset/recovery.

Compare software-reported measurements against independent instrumentation.

**Pass criteria:** software state agrees with hardware state within approved tolerances and fault transitions are deterministic.

## 13. Procedure ED13-INT — Integrated-load test

Apply the approved simultaneous workload across:

- CPU/AI compute
- network I/O
- storage I/O
- MCU telemetry
- optional FPGA fabric
- power-management functions

Monitor:

- all major rails
- current
- temperature
- clock/thermal throttling
- resets
- interface errors
- storage integrity
- security events

**Pass criteria:** no unacceptable reset, rail excursion, thermal violation, data corruption, or loss of required interface function.

## 14. Failure handling

Any failure shall be recorded with:

1. Test ID.
2. Hardware/software configuration.
3. Timestamp.
4. Instrument evidence.
5. Reproduction status.
6. Suspected cause, if known.
7. Containment action.
8. Corrective action.
9. Retest requirement.
10. Engineering disposition.

A failed test shall **not** be converted to PASS by informal observation or undocumented waiver.

## 15. Evidence package

Each completed procedure shall produce:

- signed test record;
- raw measurement data;
- waveform captures where applicable;
- instrument identifiers;
- firmware/software hashes;
- configuration/BOM identifier;
- photographs where useful;
- defect/deviation references;
- final PASS/FAIL disposition.

## 16. ED-13 exit criteria

ED-13 is complete when:

- all applicable bring-up procedures have been executed;
- power-on behavior is characterized;
- MCU and SoM boot successfully;
- required interfaces are demonstrated;
- thermal characterization is recorded;
- security-chain checks are executed;
- Nodem-SBC-OS HIL tests are recorded;
- integrated-load testing is complete;
- all failures have documented dispositions;
- evidence is traceable to the exact test article configuration.

## 17. Status

**ED-13:** PROCEDURE BASELINE ESTABLISHED.

**Execution status:** NOT YET EXECUTED — requires controlled physical prototype/test article.

**Next increment:** ED-14 test-record templates, acceptance-limit register, and traceability/evidence package structure.
