# NOD-SBC-001 — ED-12 Verification Requirements & Design Verification Matrix

**Revision:** Rev A
**Status:** Verification baseline
**Purpose:** Convert the frozen NOD-SBC-001 architecture and ED-01 through ED-11 engineering increments into a traceable design-verification program.

## 1. Verification philosophy

Verification shall demonstrate that the implemented NOD-SBC-001 design conforms to its approved electrical, mechanical, thermal, security, interface, and software requirements. Qualification is a separate activity and shall not be inferred from bench verification.

Each requirement receives a unique verification ID, verification method, objective evidence, acceptance criterion, and disposition.

**Methods:**
- **A — Analysis:** calculation/model review
- **I — Inspection:** visual/document/configuration inspection
- **T — Test:** controlled laboratory test
- **D — Demonstration:** operational demonstration
- **H — HIL:** hardware-in-the-loop integration test

## 2. Design verification matrix

| ID | Domain | Requirement / verification objective | Method | Evidence | Acceptance basis |
|---|---|---|---|---|---|
| DV-PWR-001 | Power | Verify input protection and reverse-source behavior | T | Test report + waveform capture | No unsafe reverse conduction; protected shutdown/recovery |
| DV-PWR-002 | Power | Verify power-rail nominal values and tolerance | T | Rail measurement record | All rails within approved limits |
| DV-PWR-003 | Power | Verify sequencing and reset dependencies | T | Scope captures | Required order and timing satisfied |
| DV-PWR-004 | Power | Verify overcurrent/short-circuit response | T | Fault-injection report | Protected state reached without damage |
| DV-PWR-005 | Power | Verify telemetry of voltage/current/temperature | T | Telemetry log | Measurements meet allocated accuracy |
| DV-THM-001 | Thermal | Verify SoM steady-state thermal performance | T/A | Thermal test report | Junction/case limits maintained at approved ambient |
| DV-THM-002 | Thermal | Verify thermal-spine conduction path | T | Thermal map | No uncontrolled thermal hotspot; model correlation achieved |
| DV-THM-003 | Thermal | Verify worst-case mission load envelope | T | Thermal characterization | Approved T1/T2/T3 limits satisfied |
| DV-SI-001 | SI/PI | Verify high-speed differential impedance | I/T | Stack-up + TDR report | Controlled impedance within approved design tolerance |
| DV-SI-002 | SI/PI | Verify PCIe link training and stability | T | Link test log | Required link rate/width stable under load |
| DV-SI-003 | SI/PI | Verify USB high-speed operation | T | USB compliance/functional report | Required modes enumerate and sustain load |
| DV-SI-004 | SI/PI | Verify Ethernet integrity and throughput | T | Network test report | Required link modes operate within allocation |
| DV-SI-005 | SI/PI | Verify power-integrity margins | A/T | PI report | Ripple/transient response within rail limits |
| DV-SEC-001 | Security | Verify authenticated boot chain | T | Secure-boot evidence | Unsigned/invalid image rejected |
| DV-SEC-002 | Security | Verify device identity and key isolation | T/I | Security test record | Protected identity/key material inaccessible through normal software paths |
| DV-SEC-003 | Security | Verify measured/attested platform state | T | Attestation evidence | Expected measurements reproduced and validated |
| DV-SEC-004 | Security | Verify secure firmware update/recovery | T | Update test record | Invalid update rejected; approved recovery path works |
| DV-SEC-005 | Security | Verify security lifecycle states | T | State-transition record | Unauthorized lifecycle transition rejected |
| DV-MECH-001 | Mechanical | Verify PCB/chassis datum and mounting interface | I/T | Dimensional inspection | Interfaces meet controlled drawing |
| DV-MECH-002 | Mechanical | Verify connector retention and service access | T/D | Fit/service report | Connectors survive specified insertion/service operations |
| DV-MECH-003 | Mechanical | Verify PCB isolation mounts | T | Vibration/shock fixture evidence | No structural/electrical failure within specified profile |
| DV-MECH-004 | Mechanical | Verify chassis grounding/bonding | T | Bond-resistance report | Meets approved grounding allocation |
| DV-ENV-001 | Environmental | Verify operating-temperature function | T | Environmental chamber report | Full required function throughout approved range |
| DV-ENV-002 | Environmental | Verify storage-temperature recovery | T | Environmental report | No permanent degradation after exposure |
| DV-ENV-003 | Environmental | Verify humidity/environmental sealing interfaces | T | Environmental report | Required function and enclosure interface integrity maintained |
| DV-ENV-004 | Environmental | Verify vibration/shock survivability | T | Qualification/verification report | No loss of required function or structural damage beyond allowed limits |
| DV-RF-001 | RF/EMI | Verify RF module interfaces and grounding | I/T | RF interface report | Interface and grounding requirements satisfied |
| DV-RF-002 | RF/EMI | Verify EMI/RFI shielding implementation | I/T | Shielding inspection + test | Required shielding/bonding configuration demonstrated |
| DV-SW-001 | Software | Verify Nodem-SBC-OS HAL enumerates approved hardware | HIL | HIL log | Required devices/interfaces correctly identified |
| DV-SW-002 | Software | Verify power/thermal/security telemetry APIs | HIL | API test report | Values/state transitions agree with hardware measurements |
| DV-SW-003 | Software | Verify watchdog and controlled fault recovery | HIL | Fault-injection log | Defined recovery state reached deterministically |
| DV-SW-004 | Software | Verify service/debug access controls | HIL/T | Security test report | Unauthorized service path denied |
| DV-INT-001 | Integration | Verify CPU–MCU control boundary | HIL | Integration report | Commands, status, faults, and reset behavior conform |
| DV-INT-002 | Integration | Verify optional FPGA integration boundary | HIL | FPGA integration report | FPGA-present and FPGA-absent configurations remain supported |
| DV-INT-003 | Integration | Verify storage boot and recovery | HIL/T | Boot/recovery log | Approved storage images boot; invalid image rejected |
| DV-INT-004 | Integration | Verify simultaneous compute, I/O, storage and power load | HIL/T | Stress-test report | No unacceptable reset, data corruption, thermal excursion, or rail fault |

## 3. Verification evidence package

The ED-12 evidence package shall contain, at minimum:

1. Controlled requirements-to-test trace matrix.
2. Approved schematic and PCB revision identifiers.
3. Controlled BOM revision.
4. Power-rail measurement data.
5. Thermal characterization data.
6. SI/PI analysis and measurement evidence.
7. Secure-boot and security test evidence.
8. Mechanical dimensional inspection.
9. Environmental/vibration/shock records where applicable.
10. Nodem-SBC-OS HIL test results.
11. Defect/deviation log.
12. Final verification disposition signed by the responsible engineering authority.

## 4. Entry criteria

ED-12 verification execution shall not begin against a production configuration until:

- schematic revision is controlled;
- PCB stack-up and critical constraints are controlled;
- exact component selections required for the test article are identified;
- firmware/software test baseline is identified;
- instrumentation is calibrated;
- acceptance limits are approved;
- applicable safety controls are established.

## 5. Exit criteria

ED-12 is **CLOSED** only when:

- every applicable DV requirement has a verification method;
- every executed test has objective evidence;
- all mandatory tests pass or have an approved engineering deviation;
- all failures have documented disposition;
- requirements trace to design elements and verification evidence;
- the tested configuration is reproducible from controlled artifacts;
- the verification baseline receives engineering approval.

## 6. Qualification boundary

ED-12 is a **design-verification baseline**, not a declaration of environmental qualification, regulatory compliance, or production acceptance. Qualification activities shall be separately planned against the final controlled hardware configuration.

## 7. Status

**ED-12:** VERIFICATION REQUIREMENTS BASELINE ESTABLISHED.

**Overall NOD-SBC-001 electrical-design sequence:** Architecture → compute selection → FPGA/MCU → power → thermal/mechanical/interface → security → schematic scaffold → BOM/lifecycle → exact-part selection → verification baseline.

The next increment should therefore convert the verification baseline into controlled **test procedures and quantitative acceptance limits**, beginning with power, thermal, and interface bring-up.