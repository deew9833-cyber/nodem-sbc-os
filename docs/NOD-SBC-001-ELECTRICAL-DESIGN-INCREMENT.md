# NOD-SBC-001 — Electrical Design Increment

**Revision:** Rev A implementation scaffold  
**Status:** OPEN — ED-01 recommended; detailed electrical selection and closure pending  
**Parent baseline:** NOD-SBC-001 Reference Design Rev A

## Purpose

Translate the frozen NOD-SBC-001 reference architecture into an implementable electrical-design work package without prematurely freezing production component part numbers.

## ED-01 CPU / SoM trade study

Evaluate embedded compute platforms against:

- sustained compute performance
- AI acceleration capability
- Linux/Yocto support
- secure/measured boot
- memory capacity and bandwidth
- PCIe/USB/Ethernet availability
- power consumption and thermal dissipation
- lifecycle and supply continuity
- industrial/defense environmental suitability
- software enablement and BSP maturity

**Selection status:** RECOMMENDED — Variscite DART-MX95 based on NXP i.MX95. Production part-number freeze remains pending validation. See `docs/NOD-SBC-001-ED01-CPU-SOM-TRADE-STUDY.md`.

## ED-02 FPGA / MCU

Provide a deterministic processing domain for real-time I/O, timing, watchdog/supervision, hardware acceleration, sensor aggregation, and interface bridging.

**Selection status:** OPEN pending compute-platform and I/O trade study.

## ED-03 Preliminary power architecture

```text
DC / Battery Input
        |
Input Protection / Reverse Polarity / Surge Control
        |
Power-path Management
        |
       PMIC
   +----+----+----+----+
   |    |    |    |    |
 SoC  FPGA  DDR  I/O  RF
 Rails Rails Rails Rails Rails
        |
   Power Monitor
        |
 Nodem-SBC-OS Telemetry
```

Major domains require voltage/current/temperature telemetry and defined sequencing, reset, brownout, and fault behavior.

## ED-04 Thermal architecture

```text
CPU / NPU / FPGA
        |
Copper Spreader
        |
Thermal Interface Material
        |
Aluminum Heat Plate
        |
Nodem Structural Spine
        |
Side Heat Rails
        |
External Cooling Surface
        |
Ambient Air
```

Thermal budget remains TBD until the selected SoM/FPGA and workload profile are known. The chassis spine is a controlled thermal path, not merely a mechanical member.

## ED-05 PCB envelope

Reference target: approximately **160 x 100 mm**.

This is a design target only. Final dimensions require closure of SoM, connector, thermal, creepage/clearance, routing, and enclosure constraints.

Four primary mounting points establish the board datum. Secondary attachment points may support shielding, connector retention, and thermal hardware.

## ED-06 Connector classes

- **J1 — Power:** DC/battery input, power management, service power.
- **J2 — Mission I/O:** Ethernet, USB, display/service, CAN, serial, GPIO as allocated.
- **J3 — Expansion:** PCIe, high-speed serial, RF/module interfaces, FPGA expansion.

Pin-level allocation is OPEN pending SoM and lane-budget closure.

## ED-07 High-speed interface allocation

The design shall reserve resources for:

- PCIe expansion / NVMe
- USB 3.x
- Ethernet
- display/camera MIPI where required
- high-speed SoM-to-carrier links
- FPGA high-speed I/O where required

Exact lane ownership, reference clocks, reset topology, impedance targets, and connector pin assignments are TBD pending silicon selection.

## ED-08 Security hardware

Security architecture shall provide a hardware-rooted chain including:

```text
Hardware Root of Trust
        |
Immutable / ROM Boot
        |
Verified Firmware
        |
Measured Platform State
        |
Nodem-SBC-OS
        |
Security Services
        |
Mission Applications
```

Required capabilities include device identity, secure boot, measured boot, protected key material, attestation, secure storage, cryptographic acceleration, lifecycle state, and authenticated firmware update.

PQC implementation shall be exposed through a crypto abstraction layer so algorithm/profile changes do not require pervasive application changes.

## ED-09 Nodem-SBC-OS hardware interface

The OS shall consume a stable HAL for:

- GPIO
- SPI
- I2C
- UART
- PCIe
- USB
- Ethernet
- CAN/CAN-FD
- FPGA/MCU services
- security/attestation
- storage
- thermal and power telemetry

The HAL is the principal boundary between platform-specific hardware and mission software.

## ED-10 KiCad schematic hierarchy

```text
00_top.kicad_sch
01_power/
02_compute/
03_fpga/
04_security/
05_storage/
06_network/
07_expansion/
08_rf/
09_sensor/
10_debug/
```

Each sheet shall have explicit interfaces, named power domains, net-class requirements, test points, and verification ownership.

## ED-11 Preliminary BOM classes

Initial classes:

- compute SoM
- FPGA/MCU
- PMIC/regulators
- power protection
- memory/storage
- Ethernet PHY
- security/root-of-trust device
- clocking devices
- connectors
- thermal interfaces
- EMI/RF shielding
- passives

Production manufacturer part numbers remain OPEN until the component trade studies close.

## ED-12 Verification requirements

The implementation baseline shall support verification of:

- power sequencing and fault handling
- rail accuracy and transient response
- compute boot
- secure/measured boot
- firmware authentication
- FPGA/MCU deterministic I/O
- storage integrity
- PCIe/USB/Ethernet link stability
- thermal performance under representative workloads
- power telemetry
- EMI/EMC behavior
- vibration/shock interfaces
- environmental operation
- hardware-in-the-loop software integration

## Engineering gates

| Gate | Description | Status |
|---|---|---|
| ED-01 | CPU/SoM trade | **RECOMMENDED — DART-MX95** |
| ED-02 | FPGA/MCU trade | OPEN |
| ED-03 | Power budget | OPEN |
| ED-04 | Thermal budget | OPEN |
| ED-05 | PCB envelope | TARGET ONLY |
| ED-06 | Connector pinout | OPEN |
| ED-07 | High-speed allocation | OPEN |
| ED-08 | Security hardware | ARCHITECTURE ESTABLISHED |
| ED-09 | HAL boundary | ESTABLISHED |
| ED-10 | KiCad hierarchy | ESTABLISHED |
| ED-11 | BOM | PRELIMINARY |
| ED-12 | Verification | OPEN |

## Freeze boundary

Do **not** freeze production CPU/SoM, FPGA/MCU, PMIC, connector part numbers, PCB stack-up, final dimensions, thermal hardware, or production BOM until the compute, I/O, power, thermal, signal-integrity, and lifecycle trades are reconciled.

The parent NOD-SBC-001 Rev A architecture remains the controlling baseline.
