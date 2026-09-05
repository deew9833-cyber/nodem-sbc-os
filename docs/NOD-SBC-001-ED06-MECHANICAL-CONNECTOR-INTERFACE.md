# NOD-SBC-001 — ED-06 Mechanical + Connector Interface

**Revision:** Rev A implementation increment  
**Status:** ENGINEERING BASELINE — interface targets established; production connector P/Ns remain open  
**Parent:** NOD-SBC-001 Rev A / ED-01 through ED-05

## 1. Purpose

Define the first controlled physical interface for the NOD-SBC-001 carrier so mechanical, thermal, enclosure, and electrical design can proceed without prematurely freezing production component part numbers.

## 2. PCB envelope

Reference board target remains **160 x 100 mm nominal**.

The 160 x 100 mm envelope is retained as the Rev A engineering target. It is not a production dimensional freeze until SoM, connector, thermal, routing, creepage/clearance, and enclosure checks are complete.

Recommended datum scheme:

- Primary datum A: PCB bottom plane.
- Primary datum B: long board edge.
- Primary datum C: short board edge.
- Four primary structural mounting locations establish the chassis-to-board datum.
- Secondary mounting features may be used for shielding, connector retention, and thermal hardware.

## 3. Mechanical zones

```text
┌──────────────────────────────────────────────────────────────┐
│ J2 MISSION I/O                         J3 EXPANSION           │
│                                                              │
│ ┌───────────────┐      COMPUTE / MEMORY      ┌─────────────┐ │
│ │ I/O KEEP-OUT  │                            │ RF / HS     │ │
│ └───────────────┘        ┌──────────────┐     │ KEEP-OUT    │ │
│                          │   SoM        │     └─────────────┘ │
│                          └──────────────┘                     │
│                                                              │
│                 THERMAL / STRUCTURAL ZONE                    │
│                                                              │
│ J1 POWER                    SERVICE / DEBUG                   │
└──────────────────────────────────────────────────────────────┘
```

High-speed and RF-sensitive routing shall remain separated from switching-power regions and noisy digital return paths where practical.

## 4. Connector classes

### J1 — Power

Function:

- external DC input
- battery interface
- protected power entry
- service-power input where required
- power-return reference

Requirements:

- keyed/polarized interface
- positive retention
- strain relief
- defined chassis-ground relationship
- creepage/clearance appropriate to selected input voltage

### J2 — Mission I/O

Reserved interface classes:

- Ethernet
- USB
- CAN/CAN-FD
- serial service
- GPIO
- display/service interfaces as allocated

J2 shall remain accessible in the deployed chassis service configuration.

### J3 — Expansion

Reserved interface classes:

- PCIe
- high-speed serial
- FPGA expansion
- RF/module interface
- future platform expansion

J3 shall support a keyed, mechanically retained module connection where the final application requires blind-mate service.

## 5. Connector placement rules

1. Power entry shall be physically separated from RF-sensitive interfaces.
2. High-speed connectors shall be located to minimize trace length and unnecessary vias.
3. Board-edge connectors shall have mechanical retention independent of PCB solder joints where mission vibration requirements warrant it.
4. Blind-mate interfaces shall include lead-in/chamfer features at the chassis level.
5. Service/debug access shall remain possible without removing the primary thermal interface.
6. Connector keep-outs shall be included in the PCB mechanical drawing before routing freeze.

## 6. Thermal interface

The compute thermal interface shall align with the Nodem aluminum structural spine:

```text
SoM / FPGA heat source
        │
Copper spreader
        │
TIM
        │
Aluminum heat plate
        │
Nodem structural spine
        │
Side heat rails / external cooling surface
```

The thermal interface shall be mechanically controlled by defined mounting features rather than connector preload or PCB flexure.

## 7. Chassis interface

NOD-SBC-001 shall provide:

- four primary mounting locations
- chassis-ground attachment provision
- thermal-interface mounting provision
- connector retention provisions
- service access reference
- cable/harness routing clearance
- defined exclusion zones around board edges and connectors

The hybrid polymer/aluminum Nodem chassis remains the environmental and impact-protection boundary. The PCB remains mechanically isolated from shock loads through the chassis isolation system.

## 8. Pin-level allocation status

Pin-level assignment is intentionally **not frozen** at ED-06 because the exact SoM pinout and high-speed lane ownership remain subject to ED-07.

The controlled allocation shall therefore proceed in this order:

```text
SoM pinout
   ↓
High-speed lane budget
   ↓
Connector signal groups
   ↓
Power / ground assignment
   ↓
Pin-level connector map
   ↓
PCB routing / stack-up
```

## 9. Grounding strategy

Provide distinct but intentionally bonded reference domains for:

- power return
- digital ground
- chassis ground
- RF/shield reference

The final bonding topology shall be validated during SI/PI and EMC design. The aluminum chassis is not to be treated as an uncontrolled PCB return path.

## 10. Verification

ED-06 verification shall include:

- envelope inspection
- mounting-hole positional inspection
- connector keep-out inspection
- mating/retention assessment
- blind-mate alignment assessment where applicable
- chassis thermal-interface fit check
- cable bend/strain-relief review
- chassis-ground continuity verification
- service-access inspection

## 11. Disposition

**ED-06: ENGINEERING INTERFACE BASELINE ACCEPTED.**

The 160 x 100 mm board envelope, four-point datum strategy, three connector classes, thermal-interface concept, grounding concept, and connector placement rules are established for detailed design.

Production connector manufacturer part numbers, exact pin assignments, PCB stack-up, final board dimensions, and high-speed routing remain OPEN.

## 12. Next gate

**ED-07 — High-Speed Interface Allocation**

ED-07 shall reconcile SoM lanes against PCIe/NVMe, USB 3.x, Ethernet, MIPI, FPGA, clocks, resets, and expansion requirements and produce the controlled signal/lane budget required for final connector pinout.