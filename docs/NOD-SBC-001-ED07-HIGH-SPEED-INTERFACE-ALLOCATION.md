# NOD-SBC-001 — ED-07 High-Speed Interface Allocation

**Revision:** Rev A implementation increment  
**Status:** ENGINEERING BASELINE — functional lane allocation established; exact SoM pad/pin mapping remains verification-controlled  
**Parent:** NOD-SBC-001 Rev A / ED-01 through ED-06  

## 1. Purpose

Reconcile the selected DART-MX95 / i.MX95 interface capability with the NOD-SBC-001 mission, storage, expansion, display, sensor, and service requirements before connector pin-level assignment.

This increment establishes a **functional interface budget**, not a production SoM pinout freeze. Exact SoM pad ownership, mux state, voltage domain, reference-clock topology, and carrier routing shall be verified against the released Variscite pinmux table and schematic before ED-10 KiCad implementation.

## 2. SoM interface basis

The DART-MX95 provides the relevant high-speed resources used by this allocation:

- 2 x PCIe Gen3 x1
- 1 x USB 3.0/2.0 dual-role interface
- 1 x USB 2.0 dual-role interface
- 2 x GbE plus 1 x 10GbE
- 2 x MIPI CSI-2 camera interfaces
- MIPI DSI display capability
- dual LVDS capability
- 5 x CAN-FD
- up to 8 x UART
- up to 7 x I2C plus I3C
- up to 8 x SPI
- 2 x 32-pin FlexIO interfaces
- JTAG

These capabilities are documented by Variscite and NXP. citeturn0search24turn0search1

## 3. Functional allocation

| Resource | NOD-SBC-001 allocation | Primary destination | Status |
|---|---|---|---|
| PCIe Gen3 x1 #1 | NVMe / high-speed local storage | Internal M.2 or board-mounted storage interface | **ALLOCATED** |
| PCIe Gen3 x1 #2 | Expansion fabric | J3 expansion | **ALLOCATED** |
| USB 3.0 | Mission/service high-speed USB | J2 | **ALLOCATED** |
| USB 2.0 | Low-speed USB/service/device | J2 or internal service header | **ALLOCATED** |
| 10GbE | High-bandwidth network uplink | J2/J3, application-dependent PHY/module | **RESERVED** |
| GbE #1 | Primary Ethernet | J2 | **ALLOCATED** |
| GbE #2 | Secondary Ethernet / service | J2 or J3 | **RESERVED** |
| MIPI DSI | Local display | Flip-up/removable display interface | **ALLOCATED** |
| MIPI CSI-2 #1 | Camera/sensor input | J3 or internal sensor connector | **ALLOCATED** |
| MIPI CSI-2 #2 | Expansion camera/sensor input | J3 / future expansion | **RESERVED** |
| LVDS #0/#1 | Alternate display path | Display variant only | **RESERVED** |
| CAN-FD 1–2 | Mission vehicle/sensor bus | J2 | **ALLOCATED** |
| CAN-FD 3–5 | Expansion / vehicle variant | J3 | **RESERVED** |
| FlexIO #1 | Deterministic FPGA / parallel I/O candidate | Optional FPGA interface | **RESERVED** |
| FlexIO #2 | Expansion / deterministic peripheral I/O | J3 | **RESERVED** |
| UART | Debug + supervisory/service channels | Service/debug | **ALLOCATED** |
| I2C/I3C | PMIC, sensors, EEPROM, board management | Internal | **ALLOCATED** |
| SPI | Security, control, auxiliary peripherals | Internal/J3 as required | **ALLOCATED/RESERVED** |
| JTAG | Production/debug/test | Service/debug | **ALLOCATED** |

The DART-MX95 datasheet explicitly identifies two PCIe Gen3 x1 interfaces, one USB 3.0 interface, one USB 2.0 interface, dual GbE plus 10GbE, five CAN-FD controllers, and the MIPI resources used above. citeturn0search24turn0search26

## 4. Storage architecture

### Primary storage path

**PCIe Gen3 x1 #1 → NVMe** is the default high-performance local-storage path.

The carrier shall provide the physical keep-out and reference-clock/sideband routing needed for an NVMe implementation. The final connector and mechanical implementation may use an internal M.2 device, board-mounted module, or equivalent qualified storage assembly.

### Alternate storage

The SoM-integrated eMMC remains available for boot/system storage where the selected DART-MX95 configuration includes it. SD/eMMC resources shall not consume the primary external high-speed connector allocation unless a product variant requires it.

## 5. Expansion fabric

**PCIe Gen3 x1 #2** is reserved as the primary expansion-fabric path.

J3 shall therefore provide a mechanically retained high-speed expansion interface with:

- PCIe differential pair
- reference clock
- PERST# / reset control
- wake/sideband signals where supported by the selected endpoint
- power rails
- multiple ground returns
- shield/chassis reference as required

The carrier shall not hard-wire PCIe #2 to a single mission module at ED-07. This preserves the Nodem common-carrier architecture.

## 6. Ethernet allocation

### GbE #1 — primary mission Ethernet

GbE #1 is assigned to J2 as the default mission/service Ethernet interface.

### GbE #2 — secondary Ethernet

GbE #2 remains reserved for a second physical network interface or an internal/service path depending on the final chassis variant.

### 10GbE — high-bandwidth option

The 10GbE resource is retained as a high-bandwidth expansion capability. It shall not be forced onto the common Rev-A external connector set until PHY/module selection, thermal impact, magnetics/optical interface, power budget, and SI constraints are closed.

This is a deliberate reservation rather than an omission.

## 7. USB allocation

### USB 3.0

USB 3.0 is assigned to J2 for a high-speed mission/service port.

The carrier shall preserve the USB differential-pair routing budget, ESD protection location, connector shield strategy, and controlled-impedance requirements.

### USB 2.0

USB 2.0 is assigned to J2 or an internal service interface. The final choice shall be driven by enclosure accessibility and service workflow.

## 8. Display and camera allocation

### Display

MIPI DSI is the preferred native display path for the Nodem handheld / flip-up display variant.

The ED-06 mechanical interface already establishes the need for a display/service interface while leaving the exact connector open. ED-07 now reserves the high-speed differential resource for that function.

### Camera / sensor

MIPI CSI-2 #1 is allocated as the primary camera/sensor input.

MIPI CSI-2 #2 remains reserved for a second camera or high-bandwidth sensor variant.

LVDS remains available for product variants that require the alternate display architecture. NXP documents the i.MX95 display and camera PHY resources as 4-lane interfaces, with CSI/DSI multiplexing constraints that must be preserved during pinmux validation. citeturn0search1

## 9. FPGA interface strategy

The optional FPGA remains a **variant-level capability**, not a mandatory Rev-A population.

The default deterministic FPGA control/data interface shall use FlexIO-class resources where practical. PCIe #2 remains the preferred high-bandwidth FPGA connection when the selected FPGA/module requires a packetized high-speed fabric.

Therefore:

```text
Common Rev-A:
DART-MX95 → MCU / internal control → J3 expansion

FPGA variant:
DART-MX95 → PCIe Gen3 x1 #2 → FPGA/module
                    +
                FlexIO/control
```

This preserves the common-board thermal and power envelope while allowing a higher-capability FPGA population without redesigning the core compute domain.

## 10. Clock and reset allocation

Every high-speed external fabric shall have explicit treatment for:

- reference clock source
- clock fanout/buffering where required
- reset generation
- power-good qualification
- endpoint presence/detect where applicable
- wake/interrupt sidebands
- chassis/service recovery behavior

No high-speed connector shall depend on an implicit clock or reset path.

## 11. Signal-integrity rules

Before ED-10 PCB implementation:

1. Differential-pair impedance shall be defined by the final stack-up.
2. Pair length matching shall follow the applicable SoM/interface vendor limits.
3. High-speed reference planes shall remain continuous through the route.
4. Unnecessary vias shall be avoided.
5. Power switching nodes shall be excluded from high-speed routing corridors.
6. Connector transitions shall be modeled as part of the channel where data rate requires it.
7. ESD protection shall be placed close to external connector entry while preserving channel loss and capacitance limits.
8. Chassis/shield termination shall be deliberate and shall not create uncontrolled digital return paths.

## 12. Connector-group allocation

### J1 — Power

No high-speed signals. Reserved for protected power input, returns, enable/service power, and chassis-reference provisions.

### J2 — Mission I/O

Target signal groups:

- GbE #1
- USB 3.0
- USB 2.0
- CAN-FD 1–2
- service UART
- GPIO / low-speed service
- optional display/service signals where the product variant requires external access

### J3 — Expansion

Target signal groups:

- PCIe Gen3 x1 #2
- MIPI CSI-2 #1/#2 as variant-selected
- CAN-FD 3–5
- FlexIO
- secondary GbE / 10GbE module path where justified
- FPGA control and interrupts
- power and ground

## 13. Pin-level freeze boundary

ED-07 does **not** freeze connector pin numbers.

Pin-level assignment is gated on the following evidence:

1. Exact DART-MX95 ordering configuration.
2. Released Variscite pinmux table.
3. DART-MX95 carrier reference schematic cross-check.
4. Voltage-domain verification for every selected signal.
5. MIPI mux conflict review.
6. PCIe/USB clock and reset review.
7. SI/PI preliminary review.
8. Connector P/N selection.

Variscite explicitly cautions that DART Pin2Pin compatibility depends on pinmux options and directs designers to verify the detailed pinout table for the specific SoM. citeturn0search0turn0search4

## 14. Verification matrix

| Verification | Method | Gate |
|---|---|---|
| PCIe resource allocation | SoM pinmux + schematic review | ED-07 |
| USB lane ownership | SoM pinmux review | ED-07 |
| Ethernet resource ownership | SoM schematic/pinmux review | ED-07 |
| MIPI mux conflicts | Pinmux + interface matrix | ED-07 |
| FPGA interface feasibility | FlexIO/PCIe review | ED-07 |
| Clock/reset completeness | net-level checklist | ED-07 |
| Connector group consistency | interface control review | ED-07 |
| SI route feasibility | preliminary stack-up/channel review | ED-08/ED-10 |
| Exact connector pinout | connector datasheet + pin map | ED-08/ED-10 |

## 15. Disposition

**ED-07: FUNCTIONAL HIGH-SPEED INTERFACE BASELINE ACCEPTED.**

The NOD-SBC-001 Rev A architecture now has a controlled functional allocation for PCIe, NVMe, USB, Ethernet, MIPI, CAN-FD, FPGA expansion, clocks, resets, and service interfaces.

The design intentionally avoids an unverified pad-level pinout. Exact SoM pad mapping remains a controlled verification activity against the released DART-MX95 documentation.

## 16. Next gate

**ED-08 — Security / Root-of-Trust Electrical Architecture**

ED-08 shall convert the security architecture into electrical implementation requirements covering hardware root-of-trust integration, secure boot straps, debug authorization, key-storage boundaries, tamper/status signals, lifecycle controls, and secure management interfaces.
