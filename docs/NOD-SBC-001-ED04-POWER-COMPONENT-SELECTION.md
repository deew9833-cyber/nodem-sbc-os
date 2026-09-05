# NOD-SBC-001 — ED-04 Power Component Selection

**Revision:** A.0
**Status:** Engineering selection baseline — component P/N freeze pending validation

## 1. Objective

Convert the ED-03 power topology into a componentized schematic architecture without prematurely freezing production part numbers.

## 2. Power Architecture

```text
J1 POWER INPUT
    |
    v
TVS / reverse-polarity / surge protection
    |
    v
Input eFuse / hot-swap controller
    |
    +---------------------> protected input telemetry
    |
    v
Primary buck stage
    |
    +----> +5V_MAIN --------------------+
    |                                   |
    |                              downstream loads
    |
    +----> +3V3_MAIN ------------------+
                                        |
                                   I/O / control

+5V_MAIN / +3V3_MAIN
    |
    +--> SoM point-of-load regulators
    +--> FPGA point-of-load regulators (optional)
    +--> STM32H573 rail
    +--> security device rail
    +--> storage / PHY rails

STM32H573
    |
    +--> sequencing
    +--> power-good supervision
    +--> current / voltage telemetry
    +--> thermal telemetry
    +--> controlled shutdown / recovery
```

## 3. Component Classes

| Function | Baseline class | Selection status |
|---|---|---|
| Input TVS | Automotive/industrial transient suppressor | TBD P/N |
| Reverse polarity | Ideal-diode / protected MOSFET stage | TBD P/N |
| Input protection | eFuse / hot-swap controller | TBD P/N |
| Main conversion | Synchronous buck regulator | TBD P/N |
| Point-of-load | High-efficiency buck/LDO combination | TBD P/N |
| Rail monitoring | Voltage/current monitor | TBD P/N |
| Sequencing | MCU-controlled enable/PGOOD | STM32H573 |
| Load switching | Controlled load switches | TBD P/N |
| Bulk capacitance | Polymer + MLCC | TBD values |
| EMI filtering | Common-mode / LC as required | TBD after PI testing |

## 4. Design Rules

1. Regulator continuous rating shall be at least 125% of calculated continuous load.
2. Peak transient load shall be evaluated independently from continuous rating.
3. Every mission-critical rail shall have an observable PGOOD/fault state.
4. High-current compute rails require local bulk and high-frequency decoupling.
5. Sensitive analog/RF rails shall use dedicated filtering or regulation where required by the selected devices.
6. Power domains shall support controlled sequencing and fault isolation.
7. Thermal dissipation of every regulator shall be included in the ED-05 thermal model.
8. Final regulator P/N selection requires voltage range, efficiency, switching frequency, thermal resistance, availability, lifecycle, and layout review.

## 5. Preliminary Rail Classes

| Rail | Nominal | Primary purpose |
|---|---:|---|
| VIN_PROT | input-dependent | protected source |
| +5V_MAIN | 5.0 V | carrier / downstream conversion |
| +3V3_MAIN | 3.3 V | carrier logic / I/O |
| SOM_CORE | device-specific | i.MX 95 SoM |
| SOM_MEM | device-specific | SoM memory |
| FPGA | device-specific | optional FPGA |
| MCU_3V3 | 3.3 V | STM32H573 |
| SEC_3V3 | 3.3 V | security domain |
| STORAGE | device-specific | NVMe/eMMC/SPI-NOR |
| PHY | device-specific | Ethernet / high-speed PHY |

## 6. Schematic Partition

The KiCad implementation shall use:

- `01_power_input`
- `01_power_protection`
- `01_power_main_rails`
- `01_power_point_of_load`
- `01_power_monitoring`
- `01_power_sequencing`

## 7. Freeze Boundary

This ED-04 artifact freezes the **power-tree topology and selection criteria**, not final manufacturer part numbers. Final P/N selection occurs only after the selected SoM/FPGA requirements, input-voltage range, connector allocation, thermal analysis, and availability review are reconciled.

## 8. Next Gate

**ED-05 — Thermal budget and thermal-stack closure.**

Required inputs: ED-01 SoM selection, ED-02 MCU/FPGA architecture, and ED-03 power envelope.
