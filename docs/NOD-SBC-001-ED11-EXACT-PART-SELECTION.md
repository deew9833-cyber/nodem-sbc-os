# NOD-SBC-001 — ED-11 Exact-Part Selection & Library Validation

**Baseline:** Rev A electrical-design implementation  
**Parent:** ED-10 preliminary BOM/lifecycle baseline  
**Status:** Preliminary exact-part selection; production freeze remains conditional

## 1. Objective

Convert the ED-10 candidate matrix into an engineering-control BOM with exact manufacturer ordering codes where evidence is sufficient, while explicitly retaining OPEN items where an exact part would be premature.

## 2. Exact-Part Selection

| Function | Manufacturer | Exact P/N | Package | ED-11 status |
|---|---|---|---|---|
| Application SoM | Variscite / NXP | DART-MX95 configuration TBD | SoM | **OPEN — configuration-dependent** |
| Supervisory MCU | STMicroelectronics | STM32H573RIT6 candidate | LQFP-64 | **CANDIDATE — package/pinmux verification required** |
| Input eFuse | Texas Instruments | TPS2663 family; exact suffix TBD | QFN | **OPEN — suffix/topology verification required** |
| Dual buck controller | Texas Instruments | LM5143 family; exact suffix TBD | QFN | **OPEN — regulator compensation/magnetics review required** |
| Mission load switch | Texas Instruments | TPS22965DSGR | WSON-8 | **SELECTED CANDIDATE** |
| Power monitor | Texas Instruments | INA238AIDGSR | VSSOP-10 | **SELECTED CANDIDATE** |
| Rail supervisor | Texas Instruments | TPS386000-Q1 family; exact suffix TBD | QFN | **OPEN — final rail/reset tree required** |
| Secure element | TBD | TBD | TBD | **OPEN — ED-08 security decision required** |
| FPGA | Lattice / AMD | Variant-dependent | Variant-dependent | **OPTIONAL / OPEN** |

TI identifies TPS22965DSGR as an active production device in an 8-pin WSON package, -40 to +105 °C, with 0.8–5.7 V input and 6 A maximum continuous switching capability. citeturn0search0turn0search48

TI identifies INA238AIDGSR as active/production in 10-pin VSSOP, -40 to +125 °C, with 85 V common-mode capability and I²C telemetry. citeturn1search0turn1search25

The DART-MX95 remains configuration-dependent because Variscite offers 2/4/8/16 GB LPDDR5 variants, with separate DDR configuration requirements. citeturn0search8

## 3. Library-Control Requirements

No production PCB footprint is considered released until:

- manufacturer package drawing is captured;
- pin-1 orientation is verified;
- land pattern is compared against manufacturer recommendation;
- courtyard and assembly keep-outs are checked;
- exposed-pad/via strategy is defined where applicable;
- thermal pad treatment is documented;
- symbol pin numbers match the datasheet;
- NC pins and no-connect policy are reviewed;
- ERC rules are updated;
- 3D model is linked or intentionally waived;
- high-speed package constraints are recorded.

## 4. ED-11 BOM Control Fields

```text
reference_designator
functional_block
manufacturer
manufacturer_pn
ordering_suffix
package
qualification_grade
operating_temperature
lifecycle_status
datasheet_revision
package_drawing_revision
symbol_revision
footprint_revision
approved_source
alternate_source
single_source_risk
qualification_status
production_freeze_status
```

## 5. Preliminary Controlled BOM

### Production-path candidates

1. **U_PWRMON1 — INA238AIDGSR**
2. **U_LOAD1 — TPS22965DSGR**
3. **U_MCU1 — STM32H573RIT6 candidate**
4. **U_SOM1 — DART-MX95, exact RAM/configuration TBD**

### Deliberately open

- input eFuse exact suffix;
- dual-buck controller exact suffix;
- supervisor exact suffix;
- secure element;
- Ethernet PHY;
- NVMe/eMMC/SPI-NOR exact devices;
- RF connectors;
- USB ESD/protection devices;
- FPGA variant.

## 6. Engineering Rule

An exact P/N is **not production-frozen merely because an orderable part number exists**. Electrical fit, package/library correctness, thermal/derating margin, lifecycle evidence, sourcing, security implications, and verification must all close first.

This prevents the BOM from becoming an accidental architecture freeze.

## 7. ED-11 Exit Criteria

- Exact P/Ns established for all components whose interfaces are closed.
- Open P/Ns explicitly traced to unresolved architecture or qualification inputs.
- Manufacturer package data captured for selected devices.
- KiCad symbols/footprints subjected to pin/package review.
- Preliminary controlled BOM fields defined.
- Single-source risks recorded.
- No unsupported production-availability claim made.

**ED-11 disposition: PARTIAL EXACT-PART SELECTION COMPLETE — LIBRARY VALIDATION AND REMAINING ARCHITECTURE-DEPENDENT P/N SELECTION OPEN.**

## 8. Next Gate — ED-12

Proceed to **ED-12 — NOD-SBC-001 Rev A verification requirements and design-verification matrix**, including electrical, power, thermal, signal integrity, security, mechanical, environmental, and software/HIL verification.
