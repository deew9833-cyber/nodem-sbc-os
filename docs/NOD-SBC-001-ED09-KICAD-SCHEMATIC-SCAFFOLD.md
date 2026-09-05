# NOD-SBC-001 — ED-09 KiCad Hierarchical Schematic Scaffold

**Baseline:** Rev A electrical-design implementation  
**Parent:** ED-08 security/root-of-trust architecture  
**Status:** Schematic hierarchy defined; component values and final footprints remain subject to electrical review

## 1. Objective

Convert the NOD-SBC-001 architecture into a controlled KiCad hierarchy with explicit domain boundaries and net ownership. ED-09 is a schematic-organization baseline; it does not claim PCB routing or electrical simulation closure.

## 2. Top-Level Hierarchy

```text
00_top
├── 01_power
├── 02_compute
├── 03_memory
├── 04_fpga_mcu
├── 05_security
├── 06_storage
├── 07_network
├── 08_usb_io
├── 09_rf_expansion
├── 10_sensors_timing
├── 11_debug_service
└── 12_thermal_monitoring
```

## 3. Power Sheet

```text
01_power
├── input_protection
├── sys_bus
├── buck_5v
├── buck_3v3
├── aux_rails
├── load_switches
├── current_monitoring
└── reset_supervision
```

Power-domain net names shall use explicit prefixes: `VIN_`, `SYS_`, `PWR_`, `+5V_`, `+3V3_`, `+1V8_`, `FPGA_`, and `SOM_`.

## 4. Compute / Control Sheets

`02_compute` owns the SoM connector/interface and carrier-side clocks, resets, boot controls, and power-good dependencies. Processor-core rails generated inside the SoM shall not be recreated on the carrier.

`04_fpga_mcu` owns STM32H573 management, optional FPGA interfaces, clocks, configuration, reset, and deterministic I/O.

## 5. Security Sheet

`05_security` owns secure-element interface, identity/reset signals, secure debug controls, tamper/status inputs, and security-domain power. The security sheet shall maintain the ED-08 rule that ordinary application interfaces cannot directly expose root key material.

## 6. High-Speed Sheet Boundaries

`07_network` owns Ethernet PHY and magnetics/module interface.

`08_usb_io` owns USB power switching, protection, connectors, and signal interfaces.

`09_rf_expansion` owns modular RF/mission connectors and controlled power feeds.

High-speed nets shall be named by interface and lane, e.g. `PCIE0_TX0_P/N`, `PCIE0_RX0_P/N`, `USB3_TX_P/N`, and `USB3_RX_P/N`.

## 7. Storage

`06_storage` owns NVMe/PCIe storage, eMMC, and SPI-NOR/recovery interfaces. Storage power must remain independently switchable where required for recovery and fault containment.

## 8. Debug / Service

`11_debug_service` owns JTAG/SWD/UART/service connectors and lifecycle-controlled debug enable signals. Production debug access shall remain subject to ED-08 security policy.

## 9. Annotation / ERC Rules

- Every hierarchical sheet has a unique functional prefix.
- No unlabelled cross-domain power net is permitted.
- All external connector signals receive ESD/protection review status.
- All high-speed differential pairs receive controlled-impedance attributes before PCB layout.
- All power outputs receive an owner, nominal voltage, maximum allocation, and protection method.
- ERC exceptions shall be documented rather than suppressed globally.

## 10. Required KiCad Artifacts

```text
hardware/kicad/
├── NOD-SBC-001.kicad_pro
├── NOD-SBC-001.kicad_sch
├── sym-lib-table
├── fp-lib-table
├── symbols/
├── footprints/
└── rules/
```

The initial repository scaffold may contain placeholders until exact symbols/footprints are validated against manufacturer packages.

## 11. ED-09 Exit Criteria

1. Every architectural domain has a schematic sheet.
2. Every power rail has a defined source and destination.
3. SoM, MCU, FPGA, security, storage, Ethernet, USB, RF, sensor, and debug interfaces have explicit ownership.
4. Cross-sheet nets are named and documented.
5. ERC strategy is defined.
6. No production footprint is frozen solely from a preliminary component candidate.

**ED-09 disposition: SCHEMATIC HIERARCHY ACCEPTED — DETAILED CAPTURE / SYMBOL / FOOTPRINT VALIDATION READY.**

## 12. Next Gate — ED-10

Proceed to **ED-10 — preliminary BOM and lifecycle/availability analysis**, using the component candidates established through ED-01 through ED-09 and explicitly separating candidate, alternate, and production-frozen parts.