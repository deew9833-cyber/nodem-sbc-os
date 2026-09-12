# NOD-SBC-001 ED-17 / ED-18 Physical Evidence Intake

**Control State:** ACTIVE / CONTROLLED HOLD  
**Repository:** `deew9833-cyber/nodem-sbc-os`  
**Branch:** `main`  
**Purpose:** Record the evidence boundary for physical ED-17 first-energization and ED-18 power-rail / MCU bring-up.

## 1. Current Evidence Boundary

The repository contains controlled execution packages for ED-17 and ED-18, but those packages explicitly state that physical execution has not yet been demonstrated in the repository evidence.

Accordingly, this artifact does **not** convert any ED-17 or ED-18 step to PASS, does not infer hardware execution from the existence of procedures, and does not establish prototype energization, rail measurements, MCU health, or acceptance.

## 2. Required Physical Evidence

For ED-17, intake must establish, as applicable:

- session identity and attributable prototype/configuration identifiers;
- board, PCB assembly, BOM, and revision identifiers;
- operator/reviewer and execution date/time;
- instrument identities and calibration status;
- controlled input conditions;
- DMM/current measurements and oscilloscope captures;
- power sequencing and PGOOD/reset observations;
- raw photographs and/or other attributable visual evidence;
- logs/telemetry where applicable;
- anomaly/NCR records and disposition;
- signed or otherwise attributable review/disposition records;
- a retained evidence manifest with cryptographic hashes.

For ED-18, intake must additionally establish the evidence required by the controlled procedure for power-rail characterization and supervisory MCU bring-up, including attributable firmware/software identifiers, rail voltage/current/ripple records, timing observations, MCU logs/health evidence, fault records, and instrument traceability.

## 3. Evidence Acceptance Rule

A test step may be dispositioned as PASS only when:

1. the executed configuration is attributable;
2. the applicable acceptance criterion is controlled;
3. the measurement/observation is directly evidenced;
4. required instruments and calibration are attributable;
5. raw evidence is retained and traceable;
6. anomalies are dispositioned; and
7. the result is reviewed and attributable.

Absence of evidence shall remain `NOT-RUN`, `BLOCKED`, or `OPEN` as applicable. No PASS shall be inferred from a procedure, design document, repository commit, or stated intent alone.

## 4. Intake Disposition

| Gate | Repository evidence | Physical evidence | Disposition |
|---|---|---|---|
| ED-17 first energization | Controlled package established | Not established in current repository evidence | NOT-RUN / HOLD |
| ED-18 power rail characterization | Controlled package established | Not established in current repository evidence | NOT-RUN / HOLD |
| ED-18 MCU bring-up | Controlled package established | Not established in current repository evidence | NOT-RUN / HOLD |

## 5. Release Impact

This evidence boundary is release-critical for NOD-SBC-001 hardware qualification. The repository-side documentation baseline may advance, but hardware qualification, ED-17/ED-18 PASS disposition, and downstream execution gates must remain open until attributable physical evidence is supplied and reconciled.

**Closure:** NOT CLOSED.  
**Next required evidence:** upload the ED-17/ED-18 physical test package (raw instrument exports, logs, photographs, signed records, or an equivalent attributable evidence package) for controlled reconciliation.
