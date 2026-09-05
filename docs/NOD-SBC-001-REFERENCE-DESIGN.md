# NOD-SBC-001 — Nodem™ Single Board Computer Reference Design

**Revision:** Rev A  
**Program:** Nodem™  
**Software baseline:** NOD-SBC-OS  
**Status:** Architecture/interface freeze; production component selection remains TBD

## 1. Purpose

NOD-SBC-001 is the common compute platform for Nodem™ deployment variants. The reference design separates the reusable compute module from carrier, mechanical, power, thermal, RF, and mission-specific infrastructure so processor generations can change without destabilizing the platform interface.

## 2. System architecture

```text
+--------------------------------------------------------------+
|                    NOD-SBC-001 PLATFORM                     |
+--------------------------------------------------------------+
| Compute SoM | FPGA/RT Fabric | Security / Root of Trust     |
|-------------+----------------+-------------------------------|
| DDR/Memory  | Storage        | PCIe / USB / Ethernet        |
|-------------+----------------+-------------------------------|
| RF Module   | Sensor Fabric  | Mission I/O / Expansion     |
|-------------+----------------+-------------------------------|
|                 Power + Thermal Plane                       |
+--------------------------------------------------------------+
```

### Functional domains

1. Compute
2. Deterministic fabric
3. Memory
4. Security
5. Storage
6. Communications
7. Power and thermal management

## 3. Compute architecture

The application processor is preferably implemented as a modular SoM. The Rev A architecture does not hard-freeze a processor family. Candidate classes include embedded ARM64 or x86-64 compute modules with DDR/LPDDR, secure boot, and integrated GPU/NPU or an attached accelerator.

The carrier exposes a controlled SoM interface for power, high-speed I/O, security, and management.

## 4. Deterministic processing island

An FPGA and/or MCU provides the real-time boundary alongside the application processor.

Responsibilities include:

- deterministic I/O
- sensor aggregation
- hardware packet processing
- timing services
- watchdog and supervision
- data movement
- hardware acceleration
- interface bridging
- low-latency paths

The CPU/SoM remains responsible for applications, networking, AI workloads, storage, UI, and NOD-SBC-OS services.

## 5. Security architecture

```text
Hardware Root of Trust
        |
        v
Immutable / Secure Boot
        |
        v
Verified Firmware
        |
        v
Measured Platform
        |
        v
NOD-SBC-OS
        |
        v
Security Services
        |
        v
Mission Applications
```

Required architectural capabilities:

- hardware device identity
- secure boot
- measured boot
- hardware-backed key storage
- firmware authentication
- platform attestation
- cryptographic acceleration
- trusted random generation
- lifecycle and revocation controls

Post-quantum cryptography is exposed through a crypto abstraction layer so algorithms and profiles can evolve without distributing cryptographic assumptions throughout the software stack.

## 6. Storage

| Function | Reference technology |
|---|---|
| Boot / recovery | SPI NOR |
| System / recovery OS | eMMC |
| Applications / models / mission data | NVMe |
| Serviceable storage | Removable NVMe where deployment permits |

## 7. Communications and expansion

Native interface classes:

- Ethernet
- USB / USB-C
- PCIe
- UART
- SPI
- I2C
- GPIO
- CAN / CAN-FD
- MIPI

RF and specialized mission interfaces remain modular to prevent the compute core from becoming coupled to a single radio implementation.

## 8. Power architecture

Reference connector classes:

- **J1 — Power:** battery/DC input to power management
- **J2 — Mission I/O:** Ethernet, USB, display, CAN, GPIO, serial
- **J3 — Expansion:** PCIe, high-speed serial, RF, FPGA I/O

Power sequencing, protection, monitoring, fault handling, and brownout behavior are part of the verification boundary.

## 9. Thermal architecture

```text
CPU / FPGA
    |
    v
Thermal Interface Material
    |
    v
Copper Spreader
    |
    v
Aluminum SBC Heat Plate
    |
    v
Nodem Structural Spine
    |
    v
Side Heat Rails / External Fins
    |
    v
Ambient
```

The board-to-chassis thermal interface is treated as a controlled mechanical interface rather than an incidental heatsink attachment.

## 10. Mechanical reference

Initial carrier target: approximately **160 × 100 mm** with four primary mounting holes, defined datum references, a central thermal interface, and deliberate chassis-ground points.

These dimensions are **targets, not production-frozen dimensions**. Final geometry depends on the selected SoM, connectors, enclosure, thermal budget, and service requirements.

Reference stack:

1. RF / I/O layer
2. Main PCB
3. Copper thermal plane
4. Aluminum heat spreader
5. Nodem structural spine

PCB mounting should provide mechanical isolation while preserving deliberate electrical/EMI grounding paths.

## 11. NOD-SBC-OS software boundary

```text
NOD-SBC-OS
    |
    +-- HAL
    +-- Security
    +-- Telemetry
    |
    +-- GPIO
    +-- SPI / I2C
    +-- PCIe
    +-- CPU / SoM
    +-- RF
    +-- Power
```

The HAL is the principal hardware/software stability boundary. Platform-specific drivers and implementations remain below this boundary; mission applications should consume stable NOD-SBC-OS services.

## 12. KiCad hierarchy

```text
NOD-SBC-001/
├── 00_top.kicad_sch
├── 01_power/
│   ├── input_protection
│   ├── battery
│   ├── pmic
│   └── power_monitor
├── 02_compute/
│   ├── som
│   ├── memory
│   └── boot
├── 03_fpga/
│   ├── fpga
│   ├── clock
│   └── io
├── 04_security/
│   ├── root_of_trust
│   ├── secure_element
│   └── crypto
├── 05_storage/
│   ├── nvme
│   ├── emmc
│   └── spi_nor
├── 06_network/
│   ├── ethernet
│   └── phy
├── 07_expansion/
│   ├── pcie
│   ├── usb
│   ├── can
│   └── gpio
├── 08_rf/
│   ├── rf_connector
│   └── module_interface
├── 09_sensor/
│   ├── imu
│   ├── temperature
│   └── monitoring
└── 10_debug/
    ├── jtag
    ├── uart
    └── testpoints
```

## 13. Verification boundary

| Domain | Verification objective |
|---|---|
| Mechanical | Dimensional inspection |
| Mounting | Fit/interface test |
| Power | Sequencing and fault injection |
| Thermal | Thermal characterization |
| Compute | Boot and workload testing |
| FPGA | Deterministic I/O testing |
| Storage | Read/write/endurance |
| Ethernet | Link and throughput |
| USB | Enumeration and load |
| PCIe | Link training and stability |
| Security | Secure-boot verification |
| Firmware | Signed-image verification |
| Telemetry | Sensor accuracy |
| EMI/EMC | Qualification |
| Environmental | Temperature/humidity |
| Vibration | Qualification |
| Shock | Qualification |
| Software | NOD-SBC-OS HIL testing |

## 14. Configuration identifiers

- **Program:** Nodem™
- **Hardware:** NOD-SBC-001
- **Revision:** Rev A
- **Board:** NOD-SBC-001-PCB
- **Carrier:** NOD-SBC-001-CARRIER
- **Compute module:** NOD-SBC-001-SOM
- **Mechanical interface:** NOD-MECH-IF-001
- **Power:** NOD-PWR-001
- **Thermal:** NOD-THERM-001
- **Security:** NOD-SEC-001
- **Software:** NOD-SBC-OS
- **Verification:** NOD-SBC-V&V-001

## 15. Freeze boundary

### Frozen at Rev A architecture level

- platform role and system decomposition
- compute/SoM separation
- deterministic processing island concept
- security architecture
- storage classes
- native and expansion interface classes
- NOD-SBC-OS HAL boundary
- thermal-path concept
- mechanical interface concept
- verification domains
- configuration identifier scheme

### Explicitly not frozen

- exact CPU/SoM part number
- exact FPGA/MCU part number
- PMIC selection
- connector part numbers
- final PCB dimensions
- final layer stack
- exact thermal solution and performance
- final RF module selection
- production BOM

These items require component-selection, electrical-budget, signal-integrity, thermal, lifecycle, and manufacturability analysis before production release.

## 16. Engineering disposition

**NOD-SBC-001 Rev A is an architecture/reference-design baseline, not a production-qualified hardware release.** The next engineering increment is component and interface budgeting followed by schematic capture, PCB stack-up definition, thermal analysis, BOM development, and hardware/software-in-the-loop verification planning.
