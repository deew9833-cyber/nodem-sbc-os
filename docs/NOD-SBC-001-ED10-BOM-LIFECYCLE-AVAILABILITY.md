# NOD-SBC-001 — ED-10 Preliminary BOM, Lifecycle & Availability Analysis

**Baseline:** Rev A electrical-design implementation  
**Parent:** ED-09 KiCad hierarchical schematic scaffold  
**Status:** Preliminary component control baseline; no production BOM freeze

## 1. Objective

Establish a controlled preliminary BOM for NOD-SBC-001 and separate:

1. **Candidate** — technically suitable part/family under evaluation.
2. **Alternate** — qualified fallback path with compatible function/interface to be evaluated before production release.
3. **Production-frozen** — approved exact manufacturer part number, package, grade, lifecycle status, and approved-source path.

ED-10 does **not** constitute procurement availability, second-source qualification, or final production-part approval.

## 2. Current BOM Control Classes

| Ref / Function | Candidate | Alternate / Fallback | Status | Freeze Gate |
|---|---|---|---|---|
| Application processor / SoM | Variscite DART-MX95 / NXP i.MX 95 family | AMD Kria K26I-class module for FPGA-centric variant | Candidate | SoM P/N, memory option, carrier pinout, thermal characterization, supply commitment |
| Supervisory MCU | STM32H573 industrial family | STM32H573 package variant | Candidate | Exact P/N/package, pinmux, lifecycle/availability confirmation |
| FPGA | Lattice Avant-E E30/E50 class | AMD Artix UltraScale+ class | Optional candidate | Variant-specific requirement, power/thermal budget, package/pinout, configuration-security closure |
| Input eFuse / surge protection | TI TPS2663 family | TI TPS1663 family where reverse-polarity/reverse-current requirements permit | Candidate | Exact suffix, protection topology, thermal/SOA validation |
| Main 5 V / 3.3 V buck controller | TI LM5143-class dual synchronous buck | Equivalent qualified dual-buck controller | Candidate | Exact P/N, switching-frequency plan, magnetics, compensation, EMI validation |
| Mission/USB load switch | TI TPS22965 | TI TPS22975 | Candidate | Exact P/N, current/inrush validation, thermal validation |
| Power monitor | TI INA238 | Equivalent high-side current/power monitor | Candidate | Exact P/N, shunt value, bus address, accuracy/error budget |
| Reset / rail supervisor | TI TPS386000-Q1 class | Equivalent multi-rail supervisor with watchdog | Candidate | Exact P/N, thresholds, reset timing, watchdog integration |
| Secure element | TBD | SoM hardware security facility where adequate | Open candidate | Security architecture, bus, key provisioning, lifecycle and attestation requirements |
| DDR / LPDDR | SoM-integrated / module-selected | Module-specific alternate | TBD | Exact SoM configuration and memory supply chain |
| eMMC / SPI-NOR | TBD | Qualified density/package alternates | TBD | Endurance, boot compatibility, lifecycle and sourcing |
| NVMe storage | M.2/module or mission-specific device | Qualified industrial NVMe | TBD | Capacity, endurance, temperature, power and vendor lifecycle |
| Ethernet PHY/magnetics | TBD | Qualified industrial PHY/module | TBD | MAC/PHY interface, temperature, magnetics, EMC and sourcing |
| USB protection/power | TBD | Qualified USB load switch/ESD device | TBD | Connector topology, current limit, ESD/EMC validation |
| RF/mission connectors | TBD | Qualified rugged connector family | TBD | Mechanical envelope, mating cycles, environmental and signal-integrity validation |

## 3. Lifecycle / Availability Assessment

### 3.1 Application processor / SoM

The NXP i.MX 95 family is an active platform with industrial and automotive/extended-industrial temperature options. NXP states that participating products have a minimum 10-year availability commitment, with designated automotive/telecom/medical products at 15 years. The i.MX 95 page also identifies EdgeLock Secure Enclave support and PQC capability.

The **NOD-SBC-001 production claim remains at family level only** until the exact Variscite DART-MX95 configuration and NXP processor ordering code are selected and supply is confirmed.

### 3.2 Supervisory MCU

STM32H573 industrial variants are active and in volume production. ST publishes industrial temperature options and a 10-year longevity commitment for listed variants beginning 2026-01-01. Exact package selection remains open because PCB routing, I/O count, debug, tamper, and thermal constraints have not yet been closed.

### 3.3 Input protection

TI lists TPS2663 as **ACTIVE**, with 4.5–60 V operating range, adjustable 0.6–6 A current limit, reverse-current blocking support, short-circuit protection, thermal shutdown, and -40 to +125 °C operating temperature. It is therefore a strong candidate for the protected system-input stage, but the exact suffix and external reverse-polarity FET topology remain an implementation gate.

### 3.4 Load switching

TI lists TPS22965 as **ACTIVE**, supporting up to 6 A with 0.8–5.7 V input, adjustable rise time, and -40 to +105 °C operation. TI also identifies TPS22975 as a newer active device with thermal shutdown. TPS22965 remains a candidate; TPS22975 is the preferred alternate where its thermal-protection behavior is advantageous and electrical/package review confirms compatibility.

### 3.5 Rail supervision

TI lists TPS386000-Q1 as **ACTIVE**, with four supply-monitoring capability, programmable delay, manual reset, watchdog, and -40 to +125 °C operation. It remains a candidate until the final rail count, thresholds, reset tree, and watchdog ownership are implemented and verified.

## 4. Production-Freeze Rules

A component may not enter the **production-frozen** class until all of the following are recorded:

- exact manufacturer part number and ordering suffix;
- package and assembly variant;
- temperature/quality grade;
- manufacturer lifecycle/marketing status;
- manufacturer longevity statement where available;
- at least one approved procurement path;
- second-source or explicit single-source disposition;
- datasheet and package drawing revision recorded;
- electrical stress/derating review complete;
- symbol and footprint validated against manufacturer documentation;
- pin-1 orientation and assembly constraints checked;
- PCB land pattern reviewed;
- security/lifecycle implications reviewed where applicable;
- EOL/PCN monitoring owner assigned.

## 5. Single-Source Risk Classification

| Class | Definition | Required treatment |
|---|---|---|
| A | Multiple viable qualified sources or pin-compatible alternates | Normal lifecycle monitoring |
| B | Single manufacturer but strong lifecycle evidence | Approved with active lifecycle monitoring |
| C | Single-source critical device or module | Management approval + last-time-buy/bridge strategy |
| D | Obsolescence, allocation, or unverified availability risk | Do not freeze; redesign or qualify alternate |

**Current NOD-SBC-001 disposition:** most major silicon remains **B/C risk until exact P/N and procurement evidence are recorded**. This is a control classification, not a statement that parts are currently unavailable.

## 6. SoM / FPGA Variant Strategy

The common Rev-A carrier should avoid hard-freezing an FPGA footprint unless the mission configuration requires it. The base carrier therefore preserves the ED-02 decision that the FPGA is optional.

The SoM carrier interface should remain controlled by the selected DART-MX95 configuration. A Kria/Avant/Artix implementation is treated as a variant architecture rather than an assumed drop-in alternate unless pinout, power, thermal, boot, and high-speed interfaces are proven compatible.

## 7. BOM Data Fields Required for ED-11

The controlled BOM record shall add at minimum:

```text
reference_designator
functional_block
manufacturer
manufacturer_pn
ordering_suffix
package
qualification_grade
voltage_range
current_rating
thermal_rating
lifecycle_status
longevity_commitment
approved_source
second_source
single_source_risk
symbol_revision
footprint_revision
datasheet_revision
pcn_eol_monitor_owner
qualification_status
production_freeze_status
```

## 8. ED-10 Verification Actions

1. Obtain exact manufacturer ordering codes for the SoM, MCU, power devices, and any security device.
2. Confirm current manufacturer lifecycle/marketing status at time of release.
3. Capture package drawings and recommended land patterns.
4. Establish approved distributor/contract-manufacturer sourcing paths.
5. Identify single-source critical components.
6. Record alternates without treating them as drop-in equivalents until compatibility is demonstrated.
7. Cross-check each selected part against ED-03 power allocations and ED-05 thermal limits.
8. Cross-check exact pinout/package against ED-06 mechanical/connector constraints.
9. Cross-check high-speed silicon/package capability against ED-07 allocations.
10. Cross-check security-device selection against the ED-08 root-of-trust boundary.

## 9. ED-10 Exit Criteria

- Preliminary BOM control classes established.
- Candidate/alternate/production-frozen distinction explicit.
- Lifecycle evidence recorded for major silicon candidates.
- Single-source risk identified.
- No unsupported claim of procurement availability made.
- Exact production P/Ns remain open where electrical, mechanical, security, or sourcing closure is incomplete.

**ED-10 disposition: PRELIMINARY BOM / LIFECYCLE BASELINE ACCEPTED — EXACT-P/N PROCUREMENT AND PRODUCTION-FREEZE REVIEW REMAIN OPEN.**

## 10. Next Gate — ED-11

Proceed to **ED-11 — exact-part selection, schematic symbol/footprint validation, and controlled preliminary BOM generation**, using the ED-10 candidate/alternate matrix as the input control document.
