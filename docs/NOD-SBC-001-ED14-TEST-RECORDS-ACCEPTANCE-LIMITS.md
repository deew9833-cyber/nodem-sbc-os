# NOD-SBC-001 — ED-14 Test Records, Acceptance-Limit Register & Evidence Package

**Revision:** Rev A  
**Status:** Controlled record/template baseline  
**Parent:** ED-12 Verification Requirements; ED-13 Bring-Up & Verification Procedure Set  
**Execution status:** NOT YET EXECUTED — physical test evidence is required for any PASS disposition

## 1. Purpose

ED-14 converts the ED-13 executable bring-up procedures into controlled, auditable test records. It establishes the acceptance-limit register, configuration controls, evidence naming, deviation handling, and traceability needed to support design-verification closure.

ED-14 does **not** constitute execution of ED-13. No measurement, waveform, photograph, software result, or test disposition may be represented as completed unless the underlying evidence exists and is attributable to the controlled test article.

## 2. Governing test-record chain

Every executed test shall maintain the following chain:

```text
Requirement
    ↓
Test ID
    ↓
Controlled Configuration
    ↓
Instrument / Software Tool
    ↓
Procedure Step
    ↓
Measurement / Observation
    ↓
Acceptance Limit
    ↓
Result
    ↓
Evidence
    ↓
Deviation / NCR, if applicable
    ↓
Engineering Disposition
    ↓
Approval / Closure
```

## 3. Result-state definitions

| State | Meaning |
|---|---|
| PASS | Required evidence exists and all applicable acceptance criteria are satisfied. |
| FAIL | Required evidence exists and one or more acceptance criteria are not satisfied. |
| BLOCKED | Execution cannot proceed because an approved prerequisite, configuration, instrument, fixture, or dependency is unavailable or unsuitable. |
| NOT-RUN | The test has not been executed. |
| N/A | The requirement is formally determined not applicable to the controlled configuration, with engineering rationale recorded. |

**Control rule:** `NOT-RUN`, `BLOCKED`, and `FAIL` shall never be converted to `PASS` by informal observation or undocumented waiver.

## 4. Controlled test-article configuration record

Complete before each test session.

| Field | Required entry |
|---|---|
| Test session ID | Unique identifier |
| PCB part/revision | Controlled identifier |
| PCB serial number | Exact serial |
| SoM | Exact manufacturer P/N and configuration |
| MCU | Exact P/N and silicon/revision if available |
| FPGA | Installed/not installed; exact P/N if fitted |
| BOM | Controlled revision / population record |
| Assembly revision | Controlled build identifier |
| Firmware | Version + immutable hash |
| Bootloader | Version + immutable hash |
| Nodem-SBC-OS | Version + immutable hash |
| Test software/scripts | Version + immutable hash |
| Input source | Manufacturer/model/asset ID; voltage/current limit |
| Fixture/cabling | Controlled identifiers |
| Instruments | Asset ID + calibration status |
| Ambient | Temperature / humidity |
| Operator | Name/identifier |
| Date/time | Start/end with timezone |
| Deviations | Reference IDs or `NONE` |

## 5. Acceptance-limit register

Acceptance limits shall be approved before execution. Limits may originate from the applicable controlled requirement, schematic/PCB design constraint, component specification, interface specification, thermal model, security requirement, or approved engineering analysis.

| Field | Description |
|---|---|
| AL-ID | Unique acceptance-limit identifier |
| Requirement ID | Upstream requirement reference |
| Test ID | ED-13 procedure/test reference |
| Parameter | Measured or observed parameter |
| Condition | Test mode, load, ambient, input, or other condition |
| Lower limit | Minimum permitted value, if applicable |
| Upper limit | Maximum permitted value, if applicable |
| Nominal/target | Expected value where applicable |
| Units | Controlled engineering units |
| Measurement method | Instrument, probe, software API, or observation method |
| Uncertainty/allocation | Applicable measurement uncertainty or error budget |
| Evidence type | Table, waveform, log, image, hash, report, etc. |
| Approval | Engineering authority/date |
| Revision | Acceptance-limit revision |

### 5.1 Limit-control rules

1. Limits shall be frozen before the associated test is executed.
2. A post-test limit change requires documented engineering rationale and revision control.
3. Component absolute maximum ratings are not automatically system acceptance limits.
4. Where the requirement defines a timing window, both threshold crossings and timing reference points shall be explicitly identified.
5. Where software telemetry is tested, the independent reference measurement shall be identified when required by ED-13.
6. Measurement uncertainty shall not be ignored when the margin is comparable to the instrument or method uncertainty.

## 6. Master test-record template

```text
TEST RECORD

Test ID:
Requirement ID(s):
AL-ID(s):
Procedure revision:
Test session ID:
Test article / serial:
Hardware configuration:
BOM / assembly revision:
Firmware / bootloader hash:
Nodem-SBC-OS hash:
Test software/tool hash:
Ambient conditions:
Input conditions:
Instrumentation / calibration status:
Operator:
Reviewer:
Start time:
End time:

OBJECTIVE:

PRECONDITIONS:

PROCEDURE EXECUTION:
Step | Action | Expected | Observed | Evidence ID

MEASUREMENTS:
Parameter | Nominal | Lower | Upper | Measured | Units | Instrument | Evidence

RESULT:
PASS / FAIL / BLOCKED / NOT-RUN / N/A

DEVIATION / NCR:

ENGINEERING DISPOSITION:

EVIDENCE MANIFEST:

OPERATOR APPROVAL:
REVIEWER APPROVAL:
DATE:
```

## 7. Test-group record templates

### ED14-PWR — Power-on / rail bring-up

Record:
- pre-power resistance measurements;
- source voltage/current limit;
- input voltage/current;
- every controlled rail voltage;
- rail current where instrumented;
- power-good/fault state;
- rise/fall timing;
- reset-release waveform;
- abnormal-current or physical-observation record.

Required evidence: DMM/source table, oscilloscope captures where applicable, photographs for anomalies, and deviation reference if any criterion is missed.

### ED14-SEQ — Reset & sequencing

Record:
- cold-start sequence;
- controlled shutdown;
- input interruption/recovery;
- reset assertion/deassertion timing;
- power-good ordering;
- reset cause;
- approved fault-injection result.

Required evidence: synchronized waveform captures and test log.

### ED14-MCU — MCU supervisory verification

Record:
- firmware identity/hash;
- boot result;
- clock/reset initialization;
- telemetry channel enumeration;
- voltage/current/temperature readings;
- watchdog result;
- fault-input recognition;
- controlled reset/shutdown;
- service-interface authorization result.

Where applicable, correlate MCU readings to calibrated external instrumentation.

### ED14-SOM — SoM boot verification

Record:
- exact SoM/configuration;
- boot media/image identity;
- boot timing;
- console output hash/reference;
- secure-boot state;
- memory/storage/network enumeration;
- OS health;
- reboot/power-cycle results;
- approved negative-image test result.

### ED14-THM — Thermal characterization

Record:
- ambient conditions;
- thermal instrumentation locations;
- idle baseline;
- controlled compute load;
- controlled I/O load;
- combined mission load;
- T1/T2/T3 applicability;
- component temperatures;
- clocks and throttling;
- input/rail power;
- thermal equilibrium/time limit;
- thermal-model correlation.

### ED14-IO — Ethernet / USB / PCIe

Record separately for each interface instance:
- connector/port identifier;
- negotiated mode/speed/lane width;
- enumeration;
- throughput;
- error/packet integrity metrics;
- sustained-load result;
- reset/reconnect/recovery result;
- concurrent-load condition.

### ED14-STOR — Storage verification

Record:
- storage device identity;
- firmware/configuration if applicable;
- enumeration;
- boot-path result;
- controlled read/write results;
- test-data hash/checksum;
- integrity result;
- reboot/recovery result;
- invalid-image behavior where applicable.

### ED14-SEC — Security-chain verification

Record:
- device identity reference;
- protected-key access-control result;
- authenticated-boot positive result;
- modified/invalid-image negative result;
- measured-state/attestation result;
- approved-update positive result;
- invalid-update negative result;
- recovery-policy result;
- debug/service-access control result.

For every negative security test, record the expected deny/fail-safe state and the observed state.

### ED14-HIL — Nodem-SBC-OS HIL

Record:
- HAL enumeration;
- power telemetry API result;
- thermal telemetry API result;
- security-state API result;
- MCU command/status path;
- watchdog service;
- fault reporting;
- storage/network discovery;
- reset/recovery;
- comparison of software-reported values against independent measurements where required.

### ED14-INT — Integrated-load test

Record synchronized:
- CPU/AI workload;
- network workload;
- storage workload;
- MCU telemetry;
- optional FPGA workload;
- power-management state;
- rail voltages/currents;
- temperatures;
- clocks/throttling;
- resets;
- interface errors;
- storage-integrity results;
- security events.

## 8. Instrument calibration record

Each measurement instrument shall be identified in the test record.

| Field | Entry |
|---|---|
| Instrument ID | Asset/serial |
| Manufacturer/model | Exact identifier |
| Calibration certificate | Certificate/reference |
| Calibration date | Date |
| Calibration due | Date |
| Status | In interval / exception |
| Channel/probe | Exact channel/probe |
| Measurement range | Applied range |
| Accuracy/uncertainty | Approved value |
| Operator verification | Initial/date |

An instrument outside its approved calibration interval shall not support a PASS disposition unless an approved engineering exception explicitly addresses the use.

## 9. Software and firmware baseline record

Record immutable identifiers for all executable artifacts affecting test behavior:

```text
Artifact | Version | SHA-256 | Build/source reference | Configuration
```

At minimum, identify bootloader, firmware, Nodem-SBC-OS image, HIL/test software, and any generated test configuration that materially affects results.

## 10. Evidence naming convention

Use deterministic names:

```text
<NOD-SBC-001>_<TestID>_<SessionID>_<EvidenceSeq>_<Type>_<Timestamp>.<ext>
```

Examples:

```text
NOD-SBC-001_ED13-PWR_TS001_E001_SCOPE_20260905T120000Z.png
NOD-SBC-001_ED13-MCU_TS002_E003_TELEMETRY_20260905T133000Z.csv
NOD-SBC-001_ED13-SEC_TS003_E002_BOOTLOG_20260905T150000Z.txt
```

Evidence filenames shall not be reused. Raw evidence shall be retained with the original capture where practicable; derived plots/reports shall identify their source evidence.

## 11. Deviation / nonconformance record

```text
Deviation / NCR ID:
Test ID:
Requirement ID:
Test session:
Test article / serial:
Configuration:
Observed condition:
Expected condition:
Evidence IDs:
Immediate containment:
Suspected cause:
Reproduction status:
Corrective action:
Retest required: YES / NO
Engineering disposition:
Disposition authority:
Closure evidence:
Date:
```

A deviation does not automatically invalidate a test, but its effect on requirement satisfaction shall be explicitly dispositioned.

## 12. Traceability matrix

| ED-12 verification domain | ED-13 procedure | ED-14 record family | Required evidence |
|---|---|---|---|
| Power | ED13-PWR | ED14-PWR | Rail/source measurements, waveforms |
| Sequencing | ED13-SEQ | ED14-SEQ | Timing waveforms, reset logs |
| Supervisory control | ED13-MCU | ED14-MCU | Telemetry, watchdog/fault records |
| Compute boot | ED13-SOM | ED14-SOM | Boot logs, image hashes, health results |
| Thermal | ED13-THM | ED14-THM | Temperature/power/load data |
| External I/O | ED13-IO | ED14-IO | Enumeration, throughput, error data |
| Storage | ED13-STOR | ED14-STOR | Device identity, integrity/hash data |
| Security | ED13-SEC | ED14-SEC | Positive/negative security evidence |
| Software/HIL | ED13-HIL | ED14-HIL | API/state correlation and logs |
| Integration | ED13-INT | ED14-INT | Synchronized system-load evidence |

The detailed ED-12 requirement IDs shall be inserted when the controlled ED-12 requirement identifiers are available in the repository baseline. This document does not invent missing requirement numbers.

## 13. Evidence package manifest

Each completed test session shall produce a manifest:

```text
Manifest ID:
Test session ID:
Test article serial:
Configuration ID:
Procedure revision:
Acceptance-limit revision:

Evidence ID | Filename | Type | SHA-256 | Source test step | Notes
```

The manifest shall include all raw evidence necessary to reproduce the reported disposition, plus derived artifacts used in the engineering report.

## 14. Review and approval

Minimum controlled roles:

- **Operator/Test Engineer:** confirms procedure execution and raw observations.
- **Design/Systems Engineer:** confirms technical interpretation and requirement disposition.
- **Independent reviewer/QA, where required by the project quality plan:** confirms record completeness and traceability.

Electronic approval or signature shall be attributable to the approving individual and date/time.

## 15. Evidence integrity rules

1. Preserve raw captures before processing.
2. Hash controlled digital evidence when placed into the formal evidence package.
3. Do not overwrite original evidence.
4. Record test configuration before execution.
5. Record deviations contemporaneously.
6. Tie every PASS to identifiable evidence.
7. Retain failed and blocked results; do not delete them to create a clean history.
8. If a test is repeated, issue a new session/test record and reference the prior result.

## 16. ED-14 exit criteria

ED-14 is complete as a **record/template baseline** when:

- the master test-record schema is controlled;
- acceptance-limit fields and approval rules are defined;
- all ten ED-13 test groups have record structures;
- calibration and configuration records are defined;
- firmware/software identity requirements are defined;
- evidence naming and manifest rules are defined;
- deviation/NCR handling is defined;
- ED-12 → ED-13 → ED-14 traceability structure is established;
- PASS/FAIL/BLOCKED/NOT-RUN/N/A states are controlled.

**Physical execution remains open.** ED-14 does not close ED-13 execution and does not authorize a PASS without measured evidence.

## 17. Status

**ED-14:** CONTROLLED RECORD/TEMPLATE BASELINE ESTABLISHED.

**Execution status:** NOT YET EXECUTED.

**Next engineering increment:** ED-15 controlled acceptance-limit population and test-configuration/evidence-package instantiation for the first physical NOD-SBC-001 prototype, subject to availability of the controlled prototype and approved component/configuration data.
