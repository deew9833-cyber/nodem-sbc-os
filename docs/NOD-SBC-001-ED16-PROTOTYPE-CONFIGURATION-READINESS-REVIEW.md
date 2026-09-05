# NOD-SBC-001 — ED-16 Controlled Prototype Configuration Closure & Pre-Energization Readiness Review

**Revision:** Rev A  
**Status:** Controlled readiness-review baseline  
**Parent:** ED-12 Verification Requirements; ED-13 Bring-Up & Verification Procedures; ED-14 Test Records & Acceptance-Limit Framework; ED-15 Acceptance Limits & First Prototype Test Package  
**Execution status:** NOT YET EXECUTED — no hardware energization or physical PASS is claimed by this document

## 1. Purpose

ED-16 establishes the controlled configuration-closure and pre-energization readiness review required before the first physical NOD-SBC-001 prototype may be powered.

The objective is to establish that the article, BOM, populated components, released design data, firmware/software, instrumentation, fixtures, acceptance limits, physical condition, and safety controls are mutually consistent and sufficiently controlled to authorize first energization.

ED-16 is a **readiness gate**, not a substitute for ED-13 execution. Passing the readiness review authorizes the defined first-energization boundary only; it does not constitute electrical, thermal, software, security, environmental, or system verification.

## 2. Governing sequence

```text
ED-12 Verification Requirements
        ↓
ED-13 Bring-Up & Verification Procedures
        ↓
ED-14 Test Records & Acceptance-Limit Framework
        ↓
ED-15 Acceptance Limits & First Prototype Test Package
        ↓
ED-16 Configuration Closure & Pre-Energization Readiness Review
        ↓
AUTHORIZED FIRST ENERGIZATION
        ↓
ED-13 / ED-15 TEST EXECUTION
```

An unmet ED-16 prerequisite shall result in `NO-GO`, `BLOCKED`, or `NOT-RUN` as applicable. It shall not be silently waived by proceeding to hardware testing.

## 3. Readiness status classes

| Status | Meaning |
|---|---|
| PASS | Objective evidence satisfies the applicable readiness criterion. |
| FAIL | Evidence demonstrates that the criterion is not satisfied. |
| BLOCKED | Required evidence, configuration, or prerequisite is unavailable. |
| NOT-RUN | Review item has not yet been assessed. |
| N/A | Formally determined not applicable to the controlled article. |
| CONDITIONAL | Acceptable only under an explicitly documented limitation and approval. |

## 4. Required controlled configuration identity

The following identity shall be completed before authorization:

```text
Configuration ID: NOD-SBC-001-PROT-A-CFG-001
PCB revision:
PCB serial:
Assembly revision:
Assembly serial:
BOM revision:
Schematic release identifier:
PCB layout/release identifier:
SoM manufacturer/P/N/configuration:
SoM silicon/revision information:
MCU manufacturer/P/N/silicon revision:
FPGA manufacturer/P/N/configuration:
Security-device P/N/configuration:
Storage P/N/configuration:
Power-input configuration:
Regulator P/Ns:
eFuse/protection P/Ns:
Supervisor P/N:
Firmware version/SHA-256:
Bootloader version/SHA-256:
Nodem-SBC-OS version/SHA-256:
HIL/test software version/SHA-256:
Fixture/cabling IDs:
Instrument IDs:
Environmental conditions:
Configuration authority/date:
```

### Configuration-identity rule

Every installed component that can affect power, reset, compute, security, storage, timing, thermal behavior, or externally accessible I/O shall be attributable to the controlled BOM and configuration record.

No undocumented component substitution is permitted for first-article execution.

## 5. BOM and population reconciliation

The review shall reconcile the physical article against the controlled BOM and assembly documentation.

| Check | Evidence | Result |
|---|---|---|
| BOM revision identified | Controlled BOM | NOT-RUN |
| Assembly revision identified | Assembly record | NOT-RUN |
| SoM exact P/N/configuration verified | Part marking + procurement/assembly record | NOT-RUN |
| MCU exact P/N verified | Part marking + BOM | NOT-RUN |
| FPGA option verified or formally N/A | BOM + population inspection | NOT-RUN |
| Security device verified or formally N/A | BOM + population inspection | NOT-RUN |
| Storage device verified | BOM + population inspection | NOT-RUN |
| Power-path devices verified | BOM + population inspection | NOT-RUN |
| Critical passives/components verified | BOM + assembly record | NOT-RUN |
| Population changes reconciled | ECO/deviation record if applicable | NOT-RUN |

Any discrepancy affecting electrical behavior shall be resolved or formally dispositioned before energization.

## 6. Exact-part closure gate

ED-15 identified several architecture-dependent items whose exact production part numbers remained open. ED-16 therefore requires explicit closure or controlled exclusion of those items before first energization.

The review shall specifically resolve:

- protected-input device and exact suffix;
- regulator/controller selections and exact suffixes;
- supervisor selection and exact suffix;
- exact SoM configuration;
- FPGA population decision, if applicable;
- security-device selection/configuration;
- storage population;
- connector and external power-interface population where electrically relevant.

An item may remain `TBD/TBC` only when it is demonstrably non-applicable to the physical article or when an approved configuration/deviation explicitly defines the boundary. A component required for safe or valid energization may not remain unresolved.

## 7. Released design-data review

Before authorization, confirm availability and revision identity of:

- controlled schematic;
- controlled PCB layout/fabrication release;
- controlled BOM;
- assembly drawing/population documentation;
- applicable mechanical interface/datum information;
- power-tree definition;
- reset/power-good dependency definition;
- connector/pin assignment definition;
- security configuration baseline;
- ED-13 procedure revision;
- ED-15 test-package revision;
- applicable acceptance-limit revision.

The reviewer shall verify that the physical article corresponds to the intended release and that no known ECO, erratum, or unresolved design change invalidates the first-energization procedure.

## 8. Physical inspection

The article shall undergo documented inspection before connection to the power source.

### 8.1 PCB/assembly inspection

- PCB revision and serial marking verified.
- No visible board damage, delamination, contamination, or foreign material.
- Component placement/orientation consistent with assembly documentation.
- No visible solder bridges, opens, lifted leads, damaged packages, or unpopulated critical pads inconsistent with configuration.
- Connectors fully seated and mechanically secure.
- Thermal interface components installed as specified for the controlled assembly.
- Shielding, chassis-ground, and retention features installed where applicable.
- Test points accessible as required by ED-13.

### 8.2 Mechanical inspection

- Mounting features undamaged.
- Board datum and mechanical interfaces unobstructed.
- No fastener or standoff condition capable of creating an unintended electrical short.
- Heat-spreader/spine interface is correctly assembled where applicable.
- Cable routing and strain relief do not impose unacceptable mechanical load on connectors or PCB.

### 8.3 Environmental/ESD condition

- Work area suitability confirmed.
- ESD controls active and appropriate to the article.
- Environmental conditions recorded.
- Moisture/condensation or contamination conditions that could compromise energization are absent.

## 9. Pre-energization electrical inspection

These checks are intended to detect assembly faults before controlled power is applied. They are **inspection/readiness checks**, not substitute verification of the final electrical design.

Record the instrument ID, range, measurement point, observed result, and acceptance disposition for every check.

| Check ID | Pre-energization check | Required disposition |
|---|---|---|
| PE-001 | Input connector polarity/identity verification | PASS required |
| PE-002 | Visual inspection of protected power path | PASS required |
| PE-003 | Unpowered resistance/continuity checks at designated rails | Approved limit required before PASS |
| PE-004 | Rail-to-ground isolation checks where applicable | Approved limit required before PASS |
| PE-005 | No unintended short identified on primary power domains | PASS required |
| PE-006 | Connector pinout/power-pin verification | PASS required |
| PE-007 | Power-source current limiting configured | PASS required |
| PE-008 | Emergency power disconnect/shutdown path verified | PASS required |
| PE-009 | Oscilloscope/current-probe/test-point setup ready | PASS required |
| PE-010 | First-energization sequence reviewed by operator/reviewer | PASS required |

**Important:** ED-16 does not invent resistance, isolation, current-limit, or voltage thresholds. Numerical limits shall come from the released design, applicable component/interface requirements, approved analysis, or formally approved engineering limits before the corresponding check can receive PASS.

## 10. Power-source and containment readiness

Before first energization:

- the power source shall be positively identified;
- its operating mode shall be controlled;
- current limiting/protection shall be configured using an approved test setting;
- polarity and connector mating shall be independently verified;
- the operator shall have immediate access to power removal;
- unintended external loads shall be disconnected unless explicitly required by the procedure;
- required measurement leads shall be installed before energization;
- the test area shall prevent accidental shorts or uncontrolled movement of the article;
- the approved first-energization sequence shall be available at the test station.

No first energization is authorized solely because a programmable source can be set to a nominal voltage.

## 11. Instrumentation and calibration gate

For each instrument used in the first energization or readiness checks, record:

```text
Instrument ID:
Manufacturer/model:
Measurement function:
Range/configuration:
Calibration certificate ID:
Calibration expiration/status:
Probe/accessory ID:
Measurement uncertainty, if applicable:
Operator verification:
```

Out-of-calibration equipment shall not be used for a controlled acceptance measurement unless an approved engineering/quality disposition explicitly establishes suitability for the specific measurement.

## 12. Firmware/software baseline gate

The following shall be hash-attributable before hardware execution:

- boot ROM/configuration assumptions where externally controllable;
- bootloader image;
- supervisory MCU firmware;
- FPGA bitstream where populated;
- Nodem-SBC-OS image;
- HIL/test software;
- security configuration artifacts;
- any scripts or utilities that generate or transform test evidence.

Record SHA-256 values in the configuration record and session record. A software update after readiness review invalidates the affected readiness approval unless the configuration is re-reviewed.

## 13. Acceptance-limit readiness

ED-16 shall verify that every ED-13/ED-15 criterion required for first energization has an approved status.

The following conditions are mandatory:

1. No required numerical acceptance limit is left unresolved without an approved execution boundary.
2. Limits are tied to the controlled configuration.
3. The limit revision is identified.
4. The source/rationale is traceable.
5. Measurement method and instrument capability are compatible with the limit.
6. Any conditional limit explicitly states its applicable condition.

A test may be executable with a subset of ED-15 limits only when the excluded tests are formally identified as later-stage activities and do not create an unsafe or uncontrolled first-energization condition.

## 14. Readiness review checklist

| ID | Readiness item | Evidence | Result |
|---|---|---|---|
| RR-001 | Configuration ID assigned | Configuration record | NOT-RUN |
| RR-002 | PCB/assembly revision verified | Article inspection | NOT-RUN |
| RR-003 | BOM revision verified | Controlled BOM | NOT-RUN |
| RR-004 | Physical population reconciled | Inspection/BOM reconciliation | NOT-RUN |
| RR-005 | Exact SoM configuration verified | Part/configuration record | NOT-RUN |
| RR-006 | Critical power components closed | BOM + design release | NOT-RUN |
| RR-007 | Security configuration closed | Security baseline | NOT-RUN |
| RR-008 | Released schematic available | Controlled release | NOT-RUN |
| RR-009 | Released PCB/assembly data available | Controlled release | NOT-RUN |
| RR-010 | Mechanical condition acceptable | Inspection record | NOT-RUN |
| RR-011 | Pre-energization electrical checks complete | Measurement record | NOT-RUN |
| RR-012 | Instruments calibrated | Calibration records | NOT-RUN |
| RR-013 | Firmware/software hashes recorded | Hash manifest | NOT-RUN |
| RR-014 | Acceptance-limit revision approved | Limit register | NOT-RUN |
| RR-015 | Fixture/cabling controlled | Fixture record | NOT-RUN |
| RR-016 | Environmental conditions recorded | Test log | NOT-RUN |
| RR-017 | Power-source protection configured | Test-station record | NOT-RUN |
| RR-018 | Emergency shutdown path available | Test-station check | NOT-RUN |
| RR-019 | ED-13 procedure controlled | Procedure revision | NOT-RUN |
| RR-020 | ED-15 package controlled | Test-package revision | NOT-RUN |
| RR-021 | Open deviations reviewed | Deviation/NCR register | NOT-RUN |
| RR-022 | First-energization procedure briefed | Review record | NOT-RUN |
| RR-023 | Independent reviewer concurrence obtained | Approval record | NOT-RUN |
| RR-024 | GO/NO-GO disposition recorded | Signed readiness record | NOT-RUN |

## 15. Configuration discrepancy handling

Any discrepancy shall be classified before energization:

- **Class A — safety/configuration critical:** blocks energization until resolved.
- **Class B — verification-critical:** blocks the affected test and normally blocks first energization when the discrepancy could alter electrical behavior or traceability.
- **Class C — documentation/administrative:** may proceed only with documented engineering/QA disposition and no impact to safe or valid execution.

No discrepancy may be closed by verbal agreement alone. The record shall identify the discrepancy, affected configuration, disposition authority, corrective action, and evidence.

## 16. First-energization authorization boundary

A GO disposition authorizes only the controlled first-energization operation defined by the approved ED-13 procedure.

It does **not** authorize:

- uncontrolled voltage/current excursions;
- attachment of unapproved external loads;
- firmware changes outside the controlled baseline;
- bypassing protection or safety controls;
- thermal stress beyond the approved first-step envelope;
- interface testing not covered by the active procedure;
- representation of readiness as verification PASS.

If the article behavior deviates from the approved sequence, power shall be removed as required by the procedure and the event recorded for engineering disposition.

## 17. Formal GO / NO-GO decision

### GO criteria

All applicable RR-001 through RR-024 items are PASS or formally N/A/CONDITIONAL with approval; configuration identity is complete; critical exact parts are closed; physical inspection is acceptable; required pre-energization checks have approved limits; instrumentation and fixtures are controlled; software/firmware hashes are recorded; and the operator/reviewer authorize the defined first-energization boundary.

### NO-GO criteria

A NO-GO is mandatory for any unresolved condition involving:

- article identity;
- critical component identity;
- power-path configuration;
- polarity or connector uncertainty;
- suspected short or unintended conductive path;
- unapproved acceptance limit required for the planned action;
- uncontrolled firmware/software;
- missing or unsuitable instrumentation;
- absent emergency power removal capability;
- physical damage affecting safe operation;
- unresolved safety or ESD condition;
- traceability failure;
- discrepancy that could invalidate subsequent test evidence.

## 18. Readiness review record

```text
REVIEW ID: NOD-SBC-001-ED16-RR-001
CONFIGURATION ID: NOD-SBC-001-PROT-A-CFG-001
ARTICLE SERIAL:
PCB REVISION:
ASSEMBLY REVISION:
BOM REVISION:
DATE/TIME:
LOCATION:
OPERATOR:
REVIEWER:
SYSTEMS/QA AUTHORITY:

OVERALL DISPOSITION: NOT-RUN

OPEN DISCREPANCIES:

CONDITIONAL APPROVALS:

AUTHORIZED FIRST-ENERGIZATION SCOPE:

RESTRICTIONS:

EVIDENCE MANIFEST:

OPERATOR SIGNATURE/APPROVAL:
REVIEWER SIGNATURE/APPROVAL:
SYSTEMS/QA SIGNATURE/APPROVAL:
```

## 19. Evidence requirements

The ED-16 evidence package shall contain, at minimum:

```text
ED16-READINESS-REVIEW/
├── 00_CONFIGURATION/
├── 01_BOM_POPULATION/
├── 02_RELEASED_DESIGN_DATA/
├── 03_PHYSICAL_INSPECTION/
├── 04_PRE_ENERGIZATION/
├── 05_INSTRUMENTATION_CALIBRATION/
├── 06_FIRMWARE_SOFTWARE_HASHES/
├── 07_ACCEPTANCE_LIMITS/
├── 08_DEVIATIONS_NCR/
├── 09_GO_NO_GO/
├── 10_APPROVAL/
└── 11_MANIFEST/
```

Every evidence item shall be attributable to the review/configuration and retained without overwrite. Derived records shall identify their source evidence and processing method.

## 20. Traceability

```text
ED-12 Verification Requirement
          ↓
ED-13 Verification Procedure
          ↓
ED-14 Test Record / Acceptance Framework
          ↓
ED-15 Acceptance Limit + Prototype Package
          ↓
ED-16 Configuration + Readiness Evidence
          ↓
First-Energization Authorization
          ↓
Physical Measurement / Observation
          ↓
ED-13 / ED-15 Verification Record
```

ED-16 closes the configuration/readiness boundary only. It does not close the underlying ED-12 verification requirements.

## 21. No-PASS rule

ED-16 shall never convert a readiness condition into a hardware verification result.

A readiness review marked GO means only that the defined first-energization operation is authorized under controlled conditions. Physical performance claims require subsequent execution, measurement, evidence, and review under ED-13/ED-15.

No simulated, assumed, nominal, or model-generated value may be recorded as a physical prototype observation.

## 22. Exit criteria

ED-16 is complete as a **controlled readiness-review baseline** when:

- configuration identity fields are defined;
- BOM/population reconciliation is defined;
- exact critical-part closure is explicitly required;
- released-design review is defined;
- physical inspection is defined;
- pre-energization electrical checks are defined without inventing unsupported numerical limits;
- power-source and emergency-shutdown controls are defined;
- instrumentation/calibration controls are defined;
- firmware/software hash controls are defined;
- acceptance-limit readiness is defined;
- discrepancy handling is defined;
- GO/NO-GO authorization criteria are defined;
- first-energization scope is explicitly bounded;
- evidence structure and approvals are defined;
- no physical verification PASS is falsely represented as complete.

## 23. Status

**ED-16:** CONTROLLED PROTOTYPE CONFIGURATION CLOSURE & PRE-ENERGIZATION READINESS REVIEW BASELINE ESTABLISHED.

**Execution status:** NOT YET EXECUTED.

**Next engineering increment:** ED-17 controlled first-energization session and ED-13 PWR/SEQ execution package, contingent on an executed ED-16 GO disposition.

---

**Attribution:** © 2026 Devon (Dev) F. White — Nodem™ / ManetTacSystems