# NOD-SBC-001 — ED-17 Controlled First-Energization Session & PWR/SEQ Execution Package

**Revision:** Rev A  
**Status:** Controlled execution-package baseline  
**Parent:** ED-12 Verification Requirements; ED-13 Bring-Up & Verification Procedures; ED-14 Test Records & Acceptance-Limit Framework; ED-15 Acceptance Limits & First Prototype Test Package; ED-16 Configuration Closure & Pre-Energization Readiness Review  
**Execution status:** NOT YET EXECUTED — this document does not claim hardware energization or PASS

## 1. Purpose

ED-17 defines the controlled first-energization session for the first NOD-SBC-001 prototype after an approved ED-16 GO disposition. It instantiates the initial ED-13 power and sequencing activities while preserving configuration, measurement, evidence, and stop-work controls.

ED-17 is an execution package, not evidence that the prototype has been powered. Physical results shall be entered only from an actual controlled test session.

## 2. Entry authorization

First energization shall not begin unless the active ED-16 readiness record is dispositioned **GO** for the exact article and scope of ED-17.

Required prerequisites:

- ED-16 review record complete and approved;
- exact prototype configuration identified;
- critical power-path components closed;
- approved first-step input/power limits available;
- pre-energization checks accepted;
- current-limited protected source configured;
- emergency power removal available;
- calibrated instruments connected and identified;
- firmware/software baseline hashes recorded;
- ED-13 and ED-15 revisions controlled;
- test record/session ID assigned;
- operator and reviewer present or otherwise authorized by the controlled procedure.

If any prerequisite is absent, disposition is **NO-GO / BLOCKED / NOT-RUN** as applicable.

## 3. Session identity

```text
SESSION ID: NOD-SBC-001-TS-001
ED-17 REVIEW ID: NOD-SBC-001-ED17-001
CONFIGURATION ID: NOD-SBC-001-PROT-A-CFG-001
ARTICLE SERIAL:
PCB REVISION:
ASSEMBLY REVISION:
BOM REVISION:
ED-16 APPROVAL:
ED-13 REVISION:
ED-15 REVISION:
DATE/TIME:
LOCATION:
OPERATOR:
REVIEWER:
```

## 4. Instrumentation record

```text
DC SOURCE ID / MODEL:
DMM ID / MODEL:
OSCILLOSCOPE ID / MODEL:
CURRENT MEASUREMENT ID / MODEL:
THERMAL INSTRUMENT ID / MODEL:
PROBE IDs:
CALIBRATION STATUS:
TEST FIXTURE ID:
CABLE/HARNESS ID:
```

All instruments used for acceptance measurements shall have current calibration status and suitable range/resolution for the controlled measurement.

## 5. Safety and stop conditions

Immediately stop the session and remove power according to the approved procedure if any of the following occurs:

- unexpected smoke, odor, arcing, visible damage, or abnormal heating;
- uncontrolled current behavior;
- unexpected voltage on a protected or externally accessible node;
- suspected short circuit or protection failure;
- unexpected reset/oscillation behavior that could damage the article;
- loss of required measurement visibility;
- loss of current limiting or emergency shutdown capability;
- configuration mismatch discovered after energization;
- any condition outside the approved ED-17 operating boundary.

Do not defeat protection, bypass an interlock, or continue solely to obtain a data point.

## 6. First-energization sequence

The operator shall execute the approved ED-13 procedure step-by-step and record observations contemporaneously.

### PWR-001 — Pre-source verification

Confirm source identity, polarity, programmed operating boundary, current limiting/protection, output state, measurement connections, and emergency disconnect.

**Result:** NOT-RUN

### PWR-002 — Initial article connection

Connect the controlled article using the approved interface and verify that no unapproved external load or accessory is attached.

**Result:** NOT-RUN

### PWR-003 — Initial energization

Apply power using the approved first-step source setting and observe input current, primary rail behavior, reset state, and any abnormal condition.

Record raw instrument evidence before assigning a result.

**Result:** NOT-RUN

### PWR-004 — Rail observation

Measure the released design's designated primary rails and record voltage, current, timing, and state as applicable to the approved acceptance limits.

**Result:** NOT-RUN

### PWR-005 — Power-good/reset observation

Verify the expected power-good and reset dependency defined by the released design. Capture oscilloscope evidence where required.

**Result:** NOT-RUN

### PWR-006 — Controlled shutdown

Execute the approved shutdown/removal sequence and record rail decay, reset behavior, and fault indications as applicable.

**Result:** NOT-RUN

## 7. Sequencing execution

After successful first energization within the approved boundary, execute the applicable ED-13 sequencing checks:

| Test ID | Activity | Result |
|---|---|---|
| SEQ-001 | Power-good ordering | NOT-RUN |
| SEQ-002 | Reset assertion/deassertion | NOT-RUN |
| SEQ-003 | Supervisory MCU startup | NOT-RUN |
| SEQ-004 | Controlled shutdown/restart | NOT-RUN |
| SEQ-005 | Protection/fault indication path | NOT-RUN |

Numerical timing limits shall be taken only from the approved ED-15/ED-16 acceptance-limit baseline. No value is inferred from nominal component behavior.

## 8. Raw evidence requirements

At minimum, retain as applicable:

- source settings/configuration capture;
- DMM readings;
- oscilloscope waveforms and acquisition settings;
- current measurements;
- photographs of the controlled test setup where required;
- console/serial logs;
- MCU telemetry/fault records;
- reset/power-good observations;
- operator contemporaneous notes;
- anomaly/deviation records.

Raw evidence shall be immutable after acquisition. Corrections shall be made by controlled derived records, not by overwriting raw evidence.

## 9. Measurement record

```text
MEASUREMENT ID:
TEST ID:
CONFIGURATION ID:
INSTRUMENT ID:
PROBE/ACCESSORY ID:
MEASUREMENT POINT:
RANGE/SETUP:
TIMESTAMP:
OBSERVED VALUE/STATE:
ACCEPTANCE LIMIT ID:
ACCEPTANCE LIMIT REVISION:
RAW EVIDENCE REFERENCE:
RESULT: NOT-RUN
OPERATOR:
REVIEWER:
```

## 10. Anomaly and deviation handling

Any unexpected behavior shall be recorded before further experimentation.

```text
ANOMALY/NCR ID:
TEST STEP:
CONFIGURATION:
TIME:
OBSERVATION:
INSTRUMENT/EVIDENCE:
IMMEDIATE ACTION:
POWER REMOVED: YES/NO
ENGINEERING DISPOSITION:
RETEST REQUIRED: YES/NO
APPROVAL:
```

A failed or anomalous step shall not be silently repeated under changed conditions. Any retest requires an attributable configuration and procedure basis.

## 11. Results discipline

Permitted result states are:

- **PASS** — objective evidence satisfies the approved criterion;
- **FAIL** — objective evidence does not satisfy the approved criterion;
- **BLOCKED** — required prerequisite/evidence unavailable;
- **NOT-RUN** — activity not executed;
- **N/A** — formally approved as not applicable;
- **CONDITIONAL** — accepted only under documented approved conditions.

No result may be marked PASS from expected, simulated, nominal, inferred, or model-generated behavior.

## 12. ED-17 session disposition

```text
PWR OVERALL: NOT-RUN
SEQ OVERALL: NOT-RUN

FIRST ENERGIZATION: NOT-RUN

OPEN ANOMALIES/NCRs:

RAW EVIDENCE MANIFEST:

DERIVED RESULTS:

ENGINEERING DISPOSITION:

OVERALL SESSION DISPOSITION: NOT-RUN

OPERATOR APPROVAL:
REVIEWER APPROVAL:
SYSTEMS/QA APPROVAL:
```

## 13. Exit criteria

ED-17 execution may be closed only when:

- ED-16 GO authorization is attributable to the exact article;
- first energization was executed or formally recorded as blocked/not-run;
- every executed step has attributable measurements/observations;
- acceptance limits used were controlled before execution;
- raw evidence is retained and hashed/manifested as required;
- anomalies and deviations are dispositioned or transferred to controlled open action;
- PWR and SEQ results are reviewed;
- the next ED-13 activity is explicitly authorized, blocked, or scheduled.

## 14. Traceability

```text
ED-12 Requirement
      ↓
ED-13 PWR / SEQ Procedure
      ↓
ED-14 Controlled Test Record
      ↓
ED-15 Acceptance Limits
      ↓
ED-16 Configuration + GO Authorization
      ↓
ED-17 First-Energization Session
      ↓
Raw Measurement Evidence
      ↓
Engineering Disposition
      ↓
Verification Closure
```

## 15. Current disposition

**ED-17:** CONTROLLED FIRST-ENERGIZATION EXECUTION PACKAGE ESTABLISHED.

**Hardware execution:** NOT YET EXECUTED.

**No physical PASS is claimed.**

**Next engineering increment:** ED-18 — Controlled Power-Rail Characterization & Supervisory MCU Bring-Up, contingent on ED-17 evidence and disposition.

---

**Attribution:** © 2026 Devon (Dev) F. White — Nodem™ / ManetTacSystems