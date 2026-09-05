# NOD-SBC-001 — ED-01 CPU / SoM Trade Study

**Revision:** Rev A.0  
**Status:** RECOMMENDED — selection validation required before production freeze  
**Parent baseline:** NOD-SBC-001 Electrical Design Increment Rev A  
**Decision gate:** ED-01

## 1. Objective

Select the preferred application-compute System-on-Module (SoM) for NOD-SBC-001 while preserving the architecture's modular carrier-board boundary and avoiding premature production-part-number freeze.

The candidate must support the Nodem requirements for Linux/Yocto, AI/ML acceleration, secure boot and attestation, high-speed I/O, deterministic support functions, industrial temperature operation, lifecycle continuity, and integration into the approximately 160 x 100 mm carrier target.

## 2. Candidates

| Candidate | Compute architecture | AI/ML | High-speed I/O | Real-time / deterministic | Security / lifecycle | Nodem fit |
|---|---|---|---|---|---|---|
| **Variscite DART-MX95** | NXP i.MX95; up to 6x Cortex-A55 @ 2.0 GHz + Cortex-M7 + Cortex-M33 | 2 TOPS NPU | 10GbE + 2x GbE, 2x PCIe Gen3, USB 3 | M7 + M33; retain separate FPGA/MCU for mission deterministic domain | EdgeLock Secure Enclave with PQC capability; -40 to 85 °C option; Variscite states longevity to 2039 | **PRIMARY** |
| **AMD Kria K26I** | Zynq UltraScale+ MPSoC; 4x Cortex-A53 + dual Cortex-R5F + programmable logic | FPGA/DSP acceleration; architecture-dependent | PCIe/USB/GTH; 12.5 Gb/s transceivers; multi-GbE options | **Excellent**; programmable logic + R5F | TPM 2.0; industrial -40 to 100 °C | **SECONDARY / FPGA-CENTRIC** |
| **TI AM69A** | 8x Cortex-A72 @ 2 GHz + R5F domains | 32 TOPS DLA | PCIe Gen3, multi-port Ethernet, USB 3 | Strong R5F real-time domains | Secure boot, secure storage, attestation; -40 to 105 °C | **TERTIARY / HIGH-COMPUTE** |

## 3. Candidate assessment

### 3.1 Primary — Variscite DART-MX95

The DART-MX95 is the preferred baseline for NOD-SBC-001 because it aligns unusually well with the existing architecture without forcing the carrier into an FPGA-first design.

Key characteristics documented by Variscite and NXP include:

- up to six Arm Cortex-A55 application cores at 2.0 GHz;
- Cortex-M7 and Cortex-M33 real-time/safety processors;
- 2 TOPS NPU;
- up to 16 GB LPDDR5 and 128 GB eMMC;
- 1x 10GbE plus 2x GbE;
- 2x PCIe Gen3;
- USB 3.0 plus USB 2.0;
- 5x CAN-FD at the SoC level;
- MIPI display/camera interfaces;
- industrial temperature option to -40 to 85 °C;
- SoM mechanical envelope of approximately 30 x 55 x 4.25 mm;
- Linux/Yocto support;
- product-longevity target stated by Variscite to 2039.

NXP identifies the i.MX95 EdgeLock Secure Enclave as providing hardware-rooted security capabilities including secure boot, secure update, attestation-related functions, and post-quantum cryptographic capability. This directly supports the ED-08 security architecture and reduces the amount of security infrastructure that must be added around the application processor.

The DART-MX95 therefore becomes the **preferred application-compute SoM**, while a discrete FPGA/MCU remains available as the deterministic mission-I/O and acceleration domain.

### 3.2 Secondary — AMD Kria K26I

K26I is the strongest alternative where the Nodem configuration requires substantial programmable logic, deterministic data paths, custom interfaces, or hardware acceleration.

The K26I provides approximately 256K system logic cells, 1,248 DSP slices, 144 block-RAM blocks, 64 UltraRAM blocks, four 12.5 Gb/s GTH transceivers, integrated processing-system PCIe, and programmable-logic resources. AMD specifies an industrial operating range of -40 to 100 °C and a typical/max power envelope of approximately 7.5/15 W for the listed K26I module configuration.

The principal architectural disadvantage is that it shifts the Nodem compute partition toward an FPGA/MPSoC-centric design. That is attractive for a highly deterministic node, but less aligned with the current NOD-SBC-001 requirement for a general-purpose application SoM plus independently selectable FPGA/MCU domain.

K26I remains the preferred **alternate configuration** for FPGA-heavy variants and should be retained as an electrical compatibility option during carrier planning.

### 3.3 Tertiary — TI AM69A

AM69A offers the highest stated AI acceleration of the evaluated candidates, with up to 32 TOPS through dedicated deep-learning accelerators, eight Cortex-A72 application cores at up to 2 GHz, multiple R5F real-time domains, PCIe Gen3, extensive Ethernet switching, CAN-FD, and industrial operation to -40 to 105 °C.

Its principal disadvantage for NOD-SBC-001 is architectural rather than performance-related: the device is strongly optimized around high-end vision/analytics workloads, and the current Nodem architecture does not require that degree of vision-centric integration. It also has a large 31 x 31 mm, 1414-ball package at the silicon level, increasing carrier-design complexity if used directly rather than through a suitable SoM.

AM69A should therefore remain a **high-compute alternate** for future vision/AI-heavy Nodem variants rather than the baseline Rev A carrier target.

## 4. Weighted decision matrix

Scoring scale: 1 = weak, 3 = acceptable, 5 = strong. Weights reflect the current NOD-SBC-001 architecture rather than a generic embedded-compute ranking.

| Criterion | Weight | DART-MX95 | K26I | AM69A |
|---|---:|---:|---:|---:|
| General application compute | 15% | 4 | 3 | 5 |
| AI/ML acceleration | 10% | 3 | 4 | 5 |
| Deterministic processing | 10% | 4 | 5 | 5 |
| High-speed I/O | 15% | 5 | 5 | 5 |
| Security / root of trust | 15% | 5 | 3 | 4 |
| Linux / Yocto / BSP fit | 10% | 5 | 4 | 4 |
| Thermal / power fit | 10% | 4 | 5 | 3 |
| Industrial environmental fit | 5% | 4 | 5 | 5 |
| Lifecycle / supply continuity | 5% | 5 | 4 | 4 |
| Carrier architecture fit | 5% | 5 | 4 | 3 |

**Engineering interpretation:** DART-MX95 ranks first for the present architecture because security, carrier simplicity, application compute, high-speed connectivity, and lifecycle outweigh the higher raw AI throughput of AM69A and the deeper programmable-logic integration of K26I.

The matrix is a design decision aid, not a substitute for vendor qualification, thermal characterization, signal-integrity analysis, supply-chain review, or production AVL approval.

## 5. ED-01 decision

### Recommended application SoM

**SELECT: Variscite DART-MX95, industrial-temperature configuration, based on NXP i.MX95.**

Recommended initial configuration for the electrical prototype:

- 6x Cortex-A55 class configuration;
- 8 GB LPDDR5 minimum target;
- 32 GB eMMC minimum target;
- industrial-temperature variant;
- expose both PCIe Gen3 links to the carrier where pinmux permits;
- expose 10GbE plus at least one GbE path;
- reserve USB 3 and USB 2;
- reserve CAN-FD;
- expose MIPI-DSI/CSI resources needed by the handheld/display variant;
- preserve expansion capacity for the discrete FPGA/MCU and RF subsystem;
- integrate the SoM security interface into the NOD-SEC-001 trust chain.

These are **prototype configuration targets**, not final manufacturer part-number freezes.

## 6. Carrier implications

The DART-MX95 selection changes the next electrical-design constraints as follows:

1. **J3 expansion** must reserve at least one PCIe Gen3 link for NVMe and retain a second PCIe path for mission expansion where pinmux permits.
2. **Network** must reserve the 10GbE path and at least one 1GbE path; exact PHY/module implementation remains open.
3. **USB** must preserve the SoM USB 3 capability for high-speed external I/O.
4. **Deterministic domain** remains a separate ED-02 selection. The i.MX95 M7/M33 domains do not eliminate the requirement for a selectable FPGA/MCU where mission hardware acceleration or custom deterministic interfaces require it.
5. **Security** should treat the i.MX95 EdgeLock Secure Enclave as the primary application-processor root-of-trust component while evaluating whether an additional discrete secure element is required for the Nodem device-identity and lifecycle model.
6. **Thermal** must be recalculated from the actual selected DART-MX95 configuration and workload. No production thermal rating is inferred from this trade study.
7. **Mechanical** must reserve the 30 x 55 mm SoM footprint plus keep-outs, thermal interface, carrier connectors, and service access within the 160 x 100 mm target.

## 7. Open validation items

ED-01 is **recommended, not production-frozen**. Before the carrier schematic is frozen, verify:

- exact DART-MX95 ordering configuration and silicon revision;
- production-availability status and lifecycle commitment;
- complete pinmux against NOD-SBC-001 interface requirements;
- PCIe lane/reference-clock/reset topology;
- 10GbE electrical implementation and PHY/module selection;
- measured idle, nominal, sustained, and transient power;
- SoM thermal resistance and heat-spreader interface;
- LPDDR/eMMC configuration and storage endurance;
- secure-boot and measured-boot integration with NOD-SBC-OS;
- PQC/attestation API and key-management boundary;
- EMC/EMI implications of 10GbE, PCIe, USB 3, and RF coexistence;
- second-source or alternate-SoM strategy.

## 8. Gate disposition

| Gate | Previous | New disposition |
|---|---|---|
| ED-01 CPU/SoM trade | OPEN | **RECOMMENDED — DART-MX95** |
| ED-02 FPGA/MCU trade | OPEN | OPEN — execute next |
| ED-03 Power budget | OPEN | OPEN — recalculate from selected SoM |
| ED-04 Thermal budget | OPEN | OPEN — recalculate from selected SoM |
| ED-05 PCB envelope | TARGET ONLY | TARGET ONLY — validate SoM fit |
| ED-06 Connector pinout | OPEN | OPEN — derive from pinmux/lane budget |
| ED-07 High-speed allocation | OPEN | OPEN — derive from DART-MX95 resources |
| ED-08 Security hardware | ARCHITECTURE ESTABLISHED | ARCHITECTURE ESTABLISHED — integrate i.MX95 trust chain |

## 9. Source basis

Primary vendor sources reviewed for this trade:

- NXP i.MX95 product information and security/connectivity specifications.
- Variscite DART-MX95 product page and Rev. 1.06 datasheet.
- AMD Kria K26/K26I product and data-sheet information.
- TI AM69A product and data-sheet information.

External source data is used only for the ED-01 candidate comparison. Production selection remains subject to vendor documentation and qualification at the time of design release.

## 10. Freeze boundary

ED-01 does **not** authorize production freezing of the DART-MX95 manufacturer ordering code, carrier pinout, PCB stack-up, PMIC, thermal hardware, or BOM.

The next engineering gate is **ED-02 FPGA/MCU selection**, followed by quantitative **ED-03 power-budget closure** and **ED-04 thermal-budget closure**.
