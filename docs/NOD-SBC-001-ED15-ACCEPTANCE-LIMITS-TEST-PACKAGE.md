# NOD-SBC-001 — ED-15 Controlled Acceptance-Limit Population & First Prototype Test Package

**Revision:** Rev A  
**Status:** Controlled test-package baseline  
**Parent:** ED-12 Verification Requirements; ED-13 Bring-Up & Verification Procedures; ED-14 Test Records & Acceptance-Limit Framework  
**Execution status:** NOT YET EXECUTED — no physical PASS is claimed by this document

## 1. Purpose

ED-15 instantiates the ED-14 test-record framework into a first controlled prototype test package. It defines how acceptance limits are populated, frozen, reviewed, and applied to the first physical NOD-SBC-001 article.

This increment deliberately separates **known controlled limits** from **open engineering values**. No acceptance threshold is invented merely to make a test executable. Where the current design baseline does not provide a defensible value, the limit remains `TBD` or `TBC` and the associated test remains blocked from PASS disposition until approved data exists.

## 2. Entry gate

ED-15 test execution shall not begin until all applicable items below are satisfied:

- controlled PCB/assembly revision identified;
- physical prototype serial number assigned;
- exact SoM configuration recorded;
- exact MCU and optional FPGA population recorded;
- controlled BOM revision available;
- applicable regulator/eFuse/supervisor part numbers closed;
- approved schematic/PCB release available;
- acceptance limits reviewed by the responsible engineering authority;
- calibrated instruments available;
- controlled firmware, bootloader, Nodem-SBC-OS, and test software baselines identified;
- fixture/cabling configuration identified;
- laboratory environmental conditions recorded;
- ED-13 procedure revision available;
- ED-14 record templates available.

If any mandatory prerequisite is absent, the affected test is `BLOCKED` or `NOT-RUN`, not PASS.

## 3. Acceptance-limit population method

Acceptance limits shall be derived using the following precedence:

1. Controlled system requirement.
2. Approved interface or subsystem requirement.
3. Released schematic/PCB design constraint.
4. Exact component manufacturer's applicable specification.
5. Approved thermal, power, SI/PI, or reliability analysis.
6. Approved engineering characterization limit.
7. Measurement-derived limit only after the method and rationale are formally approved.

A component absolute-maximum rating shall not be used as a system operating acceptance limit unless the requirement or engineering authority explicitly establishes that relationship.

## 4. Acceptance-limit status classes

| Status | Meaning |
|---|---|
| FROZEN | Approved numerical or categorical limit available for execution. |
| CONDITIONAL | Limit is valid only for an explicitly controlled configuration/condition. |
| TBC | Value expected to be established before execution, but source/configuration is still being closed. |
| TBD | No defensible value is presently available. Test cannot receive PASS against that criterion. |
| N/A | Formally determined not applicable to the controlled configuration. |

## 5. Initial acceptance-limit register

The following register establishes the first controlled population state without inventing unresolved values.

| AL-ID | Test | Parameter | Current status | Source / closure action |
|---|---|---|---|---|
| AL-PWR-001 | ED13-PWR | Input voltage operating range | TBC | Freeze after protected-input architecture and exact power-source interface are released. |
| AL-PWR-002 | ED13-PWR | Input current | TBC | Derive from controlled power budget and measured prototype load envelope. |
| AL-PWR-003 | ED13-PWR | +5V_MAIN voltage | TBC | Freeze from released regulator design and rail tolerance analysis. |
| AL-PWR-004 | ED13-PWR | +3V3_MAIN voltage | TBC | Freeze from released regulator design and rail tolerance analysis. |
| AL-PWR-005 | ED13-PWR | Rail rise/fall timing | TBC | Freeze from released sequencing analysis and oscilloscope measurement method. |
| AL-SEQ-001 | ED13-SEQ | Power-good ordering | FROZEN BY DESIGN INTENT / VALUE TBC | Freeze exact rail/reset dependency from released schematic. |
| AL-SEQ-002 | ED13-SEQ | Reset assertion/deassertion timing | TBC | Freeze from released reset architecture and timing budget. |
| AL-MCU-001 | ED13-MCU | MCU boot result | FROZEN | Required successful boot/initialization of the controlled supervisory firmware baseline. |
| AL-MCU-002 | ED13-MCU | Telemetry channel availability | FROZEN BY INTERFACE DEFINITION | Exact channel list shall match released schematic/firmware configuration. |
| AL-MCU-003 | ED13-MCU | Watchdog response | TBC | Freeze response-time requirement from supervisory-control design. |
| AL-SOM-001 | ED13-SOM | SoM identity/configuration | FROZEN BY CONFIGURATION | Exact manufacturer P/N and memory/storage configuration must be recorded. |
| AL-SOM-002 | ED13-SOM | Secure-boot state | FROZEN | Required state is authenticated/verified boot for the released security configuration. |
| AL-SOM-003 | ED13-SOM | Boot time | TBC | Establish requirement and measurement reference before execution. |
| AL-THM-001 | ED13-THM | Maximum component temperature | TBD | Requires approved component-specific thermal limits and exact population. |
| AL-THM-002 | ED13-THM | Thermal equilibrium criterion | TBC | Freeze test duration/stability criterion from approved thermal method. |
| AL-IO-001 | ED13-IO | Ethernet negotiated mode | TBC | Freeze per populated PHY/interface configuration. |
| AL-IO-002 | ED13-IO | USB mode/speed | TBC | Freeze per populated controller/connector implementation. |
| AL-IO-003 | ED13-IO | PCIe lane/mode | TBC | Freeze after SoM/carrier PCIe topology is released. |
| AL-STOR-001 | ED13-STOR | Storage identity/enumeration | FROZEN BY CONFIGURATION | Exact device and boot-path configuration must be recorded. |
| AL-STOR-002 | ED13-STOR | Data integrity | FROZEN | Controlled test data shall round-trip with matching hash/checksum. |
| AL-SEC-001 | ED13-SEC | Invalid-image behavior | FROZEN BY SECURITY INTENT | Invalid/untrusted image shall be denied or fail safe according to released boot policy. |
| AL-SEC-002 | ED13-SEC | Debug/service authorization | FROZEN BY SECURITY INTENT | Unauthorized access shall be denied in the released security configuration. |
| AL-HIL-001 | ED13-HIL | HAL enumeration | FROZEN BY SOFTWARE CONTRACT | Required interfaces shall enumerate according to released Nodem-SBC-OS configuration. |
| AL-HIL-002 | ED13-HIL | Telemetry correlation | TBC | Freeze correlation tolerance from measurement uncertainty and interface design. |
| AL-INT-001 | ED13-INT | Integrated mission-load stability | TBC | Freeze after controlled load definition and thermal/power envelope are released. |

**Control note:** `FROZEN` in this register means the acceptance concept is fixed; any numerical threshold not explicitly supplied by the controlled design remains TBC/TBD.

## 6. First prototype test configuration

Create one immutable configuration record before the first session:

```text
Configuration ID: NOD-SBC-001-PROT-A-CFG-001
PCB revision:
PCB serial:
Assembly revision:
BOM revision:
SoM manufacturer/P/N/configuration:
MCU manufacturer/P/N/silicon revision:
FPGA manufacturer/P/N/configuration:
Security-device P/N/configuration:
Storage P/N/configuration:
Power-input configuration:
Firmware version/SHA-256:
Bootloader version/SHA-256:
Nodem-SBC-OS version/SHA-256:
HIL/test software version/SHA-256:
Fixture/cabling IDs:
Instrument IDs/calibration status:
Environmental conditions:
Configuration authority/date:
```

No substitution of a different SoM, regulator, storage device, firmware image, or populated option shall occur without a configuration revision and impact assessment.

## 7. First article test sequence

The first prototype package shall execute, in controlled order, the following groups as applicable:

1. **ED13-PWR** — pre-power inspection and rail bring-up.
2. **ED13-SEQ** — reset, power-good, shutdown, and recovery sequencing.
3. **ED13-MCU** — supervisory firmware, telemetry, watchdog, and fault-path verification.
4. **ED13-SOM** — SoM identity, boot, secure-boot state, enumeration, and health.
5. **ED13-THM** — thermal characterization under controlled loads.
6. **ED13-IO** — Ethernet, USB, and PCIe interface verification for populated ports.
7. **ED13-STOR** — storage enumeration and integrity.
8. **ED13-SEC** — positive and approved negative security-chain tests.
9. **ED13-HIL** — Nodem-SBC-OS hardware-in-the-loop verification.
10. **ED13-INT** — integrated-load verification after subsystem prerequisites are accepted.

Failure or blockage in an upstream prerequisite shall be recorded and shall not be silently bypassed.

## 8. Instrumentation requirements

At minimum, the first prototype package shall identify and control:

- calibrated DMM(s);
- programmable DC source or controlled power input;
- oscilloscope with appropriate probes;
- current measurement capability appropriate to the expected load;
- thermal measurement instrumentation appropriate to component/package access;
- host workstation or test controller;
- network test equipment/software where Ethernet is exercised;
- controlled storage/test media;
- security/test fixtures required for approved negative testing.

Exact instrument models, asset IDs, calibration certificates, ranges, and uncertainties shall be recorded in the ED-14 instrument record before execution.

## 9. Evidence package structure

The first article evidence package shall use a deterministic structure:

```text
ED15-TEST-PACKAGE/
├── 00_CONFIGURATION/
├── 01_REQUIREMENTS/
├── 02_ACCEPTANCE_LIMITS/
├── 03_PROCEDURES/
├── 04_INSTRUMENTATION/
├── 05_RAW_EVIDENCE/
│   ├── PWR/
│   ├── SEQ/
│   ├── MCU/
│   ├── SOM/
│   ├── THM/
│   ├── IO/
│   ├── STOR/
│   ├── SEC/
│   ├── HIL/
│   └── INT/
├── 06_DERIVED_RESULTS/
├── 07_DEVIATIONS_NCR/
├── 08_MANIFEST/
└── 09_APPROVAL/
```

Raw evidence shall be retained without overwrite. Derived evidence shall reference its raw source and calculation/processing method.

## 10. First-session record skeleton

```text
SESSION ID: NOD-SBC-001-TS-001
CONFIGURATION ID: NOD-SBC-001-PROT-A-CFG-001
ARTICLE SERIAL:
DATE/TIME:
OPERATOR:
REVIEWER:

ENTRY GATE:
[ ] Configuration controlled
[ ] BOM/assembly controlled
[ ] Instruments calibrated
[ ] Firmware hashes recorded
[ ] Acceptance limits approved
[ ] ED-13 procedure revision controlled
[ ] Environment recorded
[ ] Fixture/cabling controlled

TEST RESULTS:
PWR: NOT-RUN
SEQ: NOT-RUN
MCU: NOT-RUN
SOM: NOT-RUN
THM: NOT-RUN
IO: NOT-RUN
STOR: NOT-RUN
SEC: NOT-RUN
HIL: NOT-RUN
INT: NOT-RUN

OPEN DEVIATIONS/NCRs:

SESSION DISPOSITION:

EVIDENCE MANIFEST:

OPERATOR APPROVAL:
REVIEWER APPROVAL:
```

## 11. Go / No-Go gate

### GO

All applicable entry-gate items are controlled; all executable acceptance limits are approved; instrumentation is within calibration; configuration identity is complete; safety/containment checks are complete; and the test article is authorized for energization.

### NO-GO

Any of the following requires hold/blockage:

- unresolved configuration identity;
- unapproved acceptance limit required for the test;
- unsuitable/out-of-calibration instrumentation without approved exception;
- uncontrolled firmware/software image;
- missing required fixture or safety control;
- physical damage or unexplained assembly condition;
- unresolved power-input uncertainty;
- inability to preserve required raw evidence;
- any condition where execution would invalidate traceability.

## 12. Limit-closure action register

| Action | Owner | Required evidence | Status |
|---|---|---|---|
| Close exact protected-input range | Power design | Released schematic + analysis | OPEN |
| Close exact regulator selections/suffixes | Power design | Controlled BOM | OPEN |
| Close exact SoM configuration | Compute design | Controlled configuration record | OPEN |
| Close exact FPGA population decision | FPGA design | Controlled BOM/configuration | OPEN |
| Close rail voltage tolerances | Power design | Datasheet/design analysis | OPEN |
| Close reset/PG timing limits | Digital design | Released timing budget | OPEN |
| Close thermal component limits | Thermal/reliability | Component specs + thermal analysis | OPEN |
| Close interface performance limits | I/O design | Interface requirements | OPEN |
| Close telemetry correlation tolerance | Firmware/test | Measurement uncertainty analysis | OPEN |
| Approve first-session acceptance-limit revision | Systems/QA | Signed limit register | OPEN |

## 13. Traceability

ED-15 preserves the ED-12 → ED-13 → ED-14 chain and adds the configuration/evidence instantiation layer:

```text
ED-12 Requirement
      ↓
ED-13 Procedure
      ↓
ED-14 Test Record
      ↓
ED-15 Acceptance Limit + Configuration
      ↓
Prototype Measurement / Observation
      ↓
Evidence Manifest
      ↓
Engineering Disposition
      ↓
Verification Closure
```

Detailed requirement identifiers shall be inserted from the controlled ED-12 baseline when available. This document does not invent missing requirement IDs.

## 14. No-PASS rule

ED-15 establishes the package; it does not execute the hardware.

A PASS may be assigned only when:

1. the exact test article/configuration is identified;
2. the approved procedure was executed;
3. the applicable acceptance limit was frozen before execution;
4. required measurements/observations exist;
5. required evidence is attributable to the session;
6. deviations are dispositioned;
7. the result is reviewed and approved.

No simulated, assumed, nominal, or model-generated value may be represented as a physical prototype measurement.

## 15. Exit criteria

ED-15 is complete as a **controlled test-package baseline** when:

- acceptance-limit population methodology is controlled;
- current known limits and unresolved values are explicitly classified;
- first prototype configuration record is instantiated;
- first-session record is instantiated;
- instrumentation requirements are defined;
- evidence package structure is defined;
- Go/No-Go criteria are defined;
- limit-closure actions are tracked;
- ED-12 → ED-13 → ED-14 → ED-15 traceability is established;
- no physical test result is falsely represented as complete.

## 16. Status

**ED-15:** CONTROLLED ACCEPTANCE-LIMIT / FIRST PROTOTYPE TEST-PACKAGE BASELINE ESTABLISHED.

**Physical execution:** NOT YET EXECUTED.

**Next engineering increment:** ED-16 controlled prototype configuration closure and pre-energization inspection/readiness review, followed by authorized first-article execution when the entry gate is satisfied.

---

**Attribution:** © 2026 Devon (Dev) F. White — Nodem™ / ManetTacSystems