# NOD-SBC-001 — Electrical Design Increment

**Revision:** ED-001 / Rev A development increment  
**Parent baseline:** NOD-SBC-001 Rev A architecture freeze  
**Status:** Engineering development scaffold — NOT production released

## 1. Objective

Translate the frozen NOD-SBC-001 architecture into implementable electrical-interface, budgeting, schematic, thermal, mechanical, security, BOM, and verification artifacts without prematurely freezing component selections.

## 2. Work package status

| WP | Engineering activity | Initial disposition |
|---|---|---|
| ED-01 | CPU/SoM trade study | OPEN — candidates/classes to be evaluated |
| ED-02 | FPGA/MCU selection | OPEN — deterministic-load analysis required |
| ED-03 | Detailed power budget | OPEN — baseline allocation established below |
| ED-04 | Power-tree schematic | OPEN — topology defined; parts TBD |
| ED-05 | Thermal budget | OPEN — loss estimates required after component selection |
| ED-06 | Exact PCB envelope | OPEN — constrained by SoM, connectors, thermal stack |
| ED-07 | Connector pin-level definition | OPEN — interface allocation defined; pinout TBD |
| ED-08 | High-speed interface allocation | OPEN — lane budget and SI review required |
| ED-09 | Security hardware definition | OPEN — RoT/secure-element architecture defined |
| ED-10 | KiCad hierarchical schematic scaffold | OPEN — hierarchy frozen; symbols/parts TBD |
| ED-11 | Preliminary BOM | OPEN — architecture-level BOM classes only |
| ED-12 | Rev A verification requirements | OPEN — verification domains inherited and expanded |

## 3. Design rules

1. Do not convert an architecture-level assumption into a production requirement without analysis and traceability.
2. Maintain separate budgets for nominal, peak, startup/inrush, and fault conditions.
3. Treat SoM, FPGA/MCU, connectors, thermal interface, and power stages as coupled selections.
4. Preserve the NOD-SBC-OS HAL as the hardware/software stability boundary.
5. All high-speed links require controlled-impedance routing, reference-plane continuity, length/skew constraints, and SI validation before PCB release.
6. Security functions shall have explicit trust boundaries and shall not depend solely on software policy.

## 4. Architecture-level power allocation

The following is a planning allocation, not measured consumption and not a production power rating.

| Domain | Planning share | Design note |
|---|---:|---|
| Compute SoM | 45% | Includes CPU/GPU/NPU and memory |
| FPGA/MCU | 15% | Real-time fabric and I/O |
| Storage | 8% | eMMC, SPI NOR, NVMe |
| Networking / USB / expansion | 12% | Ethernet, USB, PCIe loads |
| Security / timing / sensors | 5% | RoT, secure element, IMU, monitoring |
| RF module interface | 10% | Module-dependent; radio itself remains deployment-specific |
| Conversion / margin | 5% | Preliminary engineering allowance |

These percentages shall be replaced by measured or vendor-guaranteed rail requirements after candidate selection.

## 5. Power-tree topology

```text
DC / Battery Input
        |
        v
Input Protection + Reverse Polarity + Surge Control
        |
        v
Hot-Swap / Current Limit / Input Monitor
        |
        v
Primary Regulation / PMIC
   |       |        |       |
   v       v        v       v
Compute   FPGA     I/O    Storage
 Rails    Rails    Rails    Rails
   |       |        |       |
   +-------+--------+-------+
           |
           v
Rail Telemetry + Fault Manager
           |
           v
NOD-SBC-OS Power HAL
```

Required monitored attributes: input voltage, major rail voltage, current where practical, temperature, power-good state, brownout state, and fault latch state.

## 6. Thermal budget method

Use the following first-order accounting after component selection:

```text
P_total = P_compute + P_FPGA + P_storage + P_IO + P_RF + P_PMIC_loss
```

The thermal path remains:

```text
Silicon
  -> TIM
  -> Copper spreader
  -> Aluminum heat plate
  -> Nodem structural spine
  -> Side heat rails / fins
  -> Ambient
```

Thermal release requires junction-temperature limits, ambient operating range, interface resistance, chassis resistance, and steady-state/transient characterization.

## 7. Interface allocation

| Interface class | Primary destination | Status |
|---|---|---|
| PCIe | NVMe / expansion / accelerator | Lane budget TBD |
| USB 3.x | external/service I/O | Lane budget TBD |
| Ethernet | PHY / magnetics / connector | PHY TBD |
| MIPI | display/camera class devices | Lane budget TBD |
| UART | debug/management/mission serial | Allocation TBD |
| SPI | secure element/sensors/control | Allocation TBD |
| I2C | PMIC/sensors/management | Allocation TBD |
| CAN-FD | vehicle/mission bus | Transceiver TBD |
| GPIO | controls/interrupts | Allocation TBD |
| JTAG | FPGA/SoM debug | Production access policy TBD |
| RF module | modular communications | Connector/module TBD |

## 8. Security hardware definition

Minimum architecture:

```text
Boot ROM / SoM secure boot
          |
          v
Hardware Root of Trust
          |
          +--> Device identity
          +--> Key protection
          +--> Firmware verification
          +--> Measured boot / attestation
          +--> TRNG / crypto services
          |
          v
NOD-SBC-OS security services
```

PQC algorithms remain behind a crypto abstraction layer. Exact secure-element/TPM-class implementation is a component-selection decision and remains TBD.

## 9. PCB envelope decision rule

The prior 160 × 100 mm target remains a planning envelope only. The production envelope shall be derived from:

- selected SoM keepouts and connector stack;
- memory/storage placement;
- high-speed routing topology;
- power-stage thermal dissipation;
- central chassis thermal interface;
- mounting-hole and datum requirements;
- RF keepouts;
- service access;
- manufacturing panelization and DFM constraints.

## 10. KiCad implementation scaffold

```text
NOD-SBC-001/
├── 00_top
├── 01_power
├── 02_compute
├── 03_fpga
├── 04_security
├── 05_storage
├── 06_network
├── 07_expansion
├── 08_rf
├── 09_sensor
└── 10_debug
```

Each block shall receive explicit power, ground, clock, reset, enable, interrupt, and interface ownership before schematic release.

## 11. Preliminary BOM classes

- Compute SoM
- FPGA or real-time MCU
- secure root-of-trust / secure element
- PMIC and point-of-load regulators
- input protection / hot-swap controller
- power monitors
- SPI NOR
- eMMC
- NVMe device or connector
- Ethernet PHY and magnetics
- USB interface components
- CAN-FD transceiver
- clock generators/oscillators
- temperature/voltage/current sensors
- connectors and board-to-board interconnects
- thermal spreader/interface materials
- PCB

No manufacturer part number is production-frozen by this increment.

## 12. Verification expansion

Before hardware release, demonstrate at minimum:

- power sequencing and brownout behavior;
- rail regulation under nominal and peak loads;
- startup/inrush and fault-current behavior;
- CPU/FPGA thermal limits across defined ambient conditions;
- PCIe link training and sustained operation;
- USB enumeration and load testing;
- Ethernet link/throughput/stability;
- storage integrity and endurance characterization;
- secure boot and signed-image rejection tests;
- hardware-backed identity/key-access tests;
- telemetry accuracy and fault reporting;
- SI/PI review for high-speed interfaces;
- EMI/EMC pre-compliance before qualification;
- mechanical fit, grounding, and thermal-interface validation;
- environmental, vibration, and shock qualification as applicable;
- NOD-SBC-OS hardware-in-the-loop coverage.

## 13. Exit criteria for electrical design release

Electrical Design Increment ED-001 may advance toward schematic/PCB release only when:

1. SoM and real-time processor selections have documented trade studies.
2. All power rails have voltage/current/tolerance/sequencing requirements.
3. Worst-case power and thermal budgets close with margin.
4. Connector pinout and ownership are baselined.
5. High-speed lane allocation passes preliminary SI/PI review.
6. Security hardware trust boundaries are documented.
7. PCB envelope and stack-up are compatible with mechanical/thermal interfaces.
8. BOM risks, lifecycle, and second-source strategy are recorded.
9. Verification requirements are traceable to interfaces and requirements.

**Disposition:** Architecture remains frozen at Rev A. Electrical implementation is now an open, traceable engineering increment; no production component has been silently frozen.
