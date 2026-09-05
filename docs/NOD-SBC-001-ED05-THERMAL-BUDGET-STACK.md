# NOD-SBC-001 — ED-05 Thermal Budget & Thermal-Stack Closure

**Revision:** A.0  
**Status:** Engineering thermal baseline — qualification pending measured characterization

## 1. Objective

Convert the ED-03 power envelope and ED-04 power-tree architecture into a board/chassis thermal model and establish the thermal-stack requirements for NOD-SBC-001.

ED-05 freezes the **thermal architecture and allocation method**, not final component junction temperatures, heatsink dimensions, or production thermal limits. Those remain dependent on the selected SoM/FPGA configuration, regulator P/Ns, PCB stackup, enclosure geometry, ambient conditions, and prototype measurements.

## 2. Thermal Architecture Decision

The Nodem chassis shall use the aluminum structural core as the primary passive heat path for concentrated compute and conversion losses.

```text
CPU / FPGA / HOT REGULATOR
          |
          v
 local copper heat spreader
          |
          v
 controlled-compression TIM
          |
          v
 aluminum heat plate / thermal interface block
          |
          v
 Nodem 6061-T6 structural spine
          |
          +----> side heat rails
          |          |
          |          v
          |      external fins / chassis surface
          |
          v
      ambient air
```

### Thermal architecture rules

1. High-power heat sources shall have a defined conductive path to the aluminum structural core.
2. Copper spreaders shall be used where required to reduce local thermal gradients and distribute heat into the chassis interface.
3. TIM interfaces shall be mechanically controlled; uncontrolled air gaps are not acceptable at primary thermal interfaces.
4. The structural spine shall remain electrically bonded to chassis ground where required by the system EMC/grounding architecture, while electrical isolation at component interfaces shall be maintained where necessary.
5. Thermal paths shall not depend on polymer armor panels as the primary heat sink.
6. Battery cells and temperature-sensitive components shall be physically separated from concentrated processor/regulator heat sources.
7. Thermal expansion and differential CTE between PCB, copper spreader, TIM, and aluminum structure shall be considered in the mechanical design.
8. The final design shall permit service/removal of the PCB or thermal cartridge without damaging the primary thermal interface.

## 3. Power-to-Heat Model

For each conversion stage:

`P_loss = P_load × (1/η − 1)`

where `η` is the measured or vendor-specified efficiency at the applicable operating point.

Total board heat shall be modeled as:

`P_BOARD_HEAT = P_SOM + P_FPGA + P_MCU + P_STORAGE + P_PHY + P_CONVERSION_LOSS + P_AUX`

External loads whose heat is not deposited in the enclosure shall be excluded from board dissipation and tracked separately.

For a first-order thermal path:

`T_hot = T_ambient + P × R_theta_total`

with:

`R_theta_total = R_source-interface + R_TIM + R_spreader + R_chassis + R_chassis-ambient`

The individual resistances shall be replaced by measured or validated values during prototype thermal characterization.

## 4. Thermal Classes

ED-03 established three board-dissipation classes. ED-05 retains those classes for mechanical and thermal design partitioning.

| Class | Board dissipation | Thermal intent |
|---|---:|---|
| T1 | ≤10 W | Low-power/headless configuration |
| T2 | >10 W to 20 W | Common mission SBC configuration |
| T3 | >20 W | FPGA/RF/high-throughput variant |

The preliminary expanded mission envelope can approach 32 W system input allocation; this does **not** mean 32 W is necessarily dissipated on the PCB. ED-05 requires the electrical budget to be converted into actual local heat dissipation after regulator efficiency and external-load power are accounted for.

## 5. Preliminary Thermal Allocation

The following is an engineering allocation for placement and thermal-path planning, not a measured loss budget.

| Heat source | Preliminary thermal allocation | Primary path |
|---|---:|---|
| i.MX 95 SoM | 8–12 W | copper spreader → TIM → aluminum spine |
| STM32H573 + management circuitry | ≤0.5 W | PCB copper → local chassis conduction |
| NVMe/storage | up to 5 W | PCB copper / local spreader as required |
| Ethernet PHY | up to 2 W | PCB copper → chassis/interface as required |
| USB/power conversion | load-dependent | regulator copper → chassis; externally delivered power excluded from board heat except conversion loss |
| Optional FPGA | up to 5 W allocation | dedicated spreader → TIM → aluminum spine |
| Main power conversion losses | TBD from selected efficiency curves | regulator copper → chassis / ambient |
| RF/mission modules | mission-dependent | module-specific thermal path |
| Sensors/timing | ≤1 W allocation | PCB copper / ambient |

These allocations inherit the preliminary ED-03 electrical envelope and shall not be treated as component maximum ratings.

## 6. Baseline Rev-A Thermal Case

The common Rev-A population is defined as:

- i.MX 95 application-processing SoM
- STM32H573 management MCU
- storage
- Ethernet/networking
- USB and mission I/O as populated
- optional FPGA omitted unless the variant explicitly enables it
- optional display load excluded from the headless SBC thermal baseline

The design objective is to keep the common configuration within **T2 (≤20 W board dissipation)** under the defined continuous workload after regulator losses are included.

If characterization places the common configuration above 20 W board dissipation, the configuration shall be reclassified as T3 and the chassis thermal solution shall be reviewed before release.

## 7. High-Power Variant Thermal Case

The optional FPGA, high-throughput storage/networking, and RF mission loads shall be treated as a T3 variant when their combined local dissipation exceeds 20 W.

T3 shall require:

- dedicated thermal spreading for the dominant heat source(s)
- verified thermal interface compression and flatness
- direct conductive path into the aluminum spine
- thermal isolation of battery and temperature-sensitive devices
- enclosure surface-temperature review
- worst-case ambient characterization
- sustained-load testing rather than short-duration benchmark testing alone

A high-performance FPGA population shall not be assumed thermally compatible with the common carrier merely because it fits the electrical connector/pin allocation.

## 8. Thermal Stack Requirements

The preliminary physical stack is:

1. Processor/FPGA package or module heat source
2. local copper spreader or heat plate
3. controlled TIM interface
4. aluminum thermal plate/interface block
5. 6061-T6 structural spine
6. side heat rails/cross-members
7. external chassis surface and optional fins
8. ambient convection and radiation

### Interface requirements

- Mating surfaces shall be flat enough to support the selected TIM technology.
- TIM thickness shall be minimized while accommodating manufacturing tolerance and mechanical compliance.
- Fastener preload shall be controlled where the thermal interface depends on compression.
- Thermal interface materials shall be selected for the final temperature range, aging, compression set, and service requirements.
- Thermal interfaces shall be documented as replaceable service items where practical.

## 9. PCB Thermal Design

The PCB shall provide low-impedance thermal spreading beneath and around concentrated heat sources.

Required layout practices:

1. Use solid copper planes or copper regions under high-dissipation devices where permitted by the component package and signal-integrity constraints.
2. Use thermal vias to connect exposed pads and heat-spreading copper to internal copper planes where supported by the component datasheet.
3. Keep switching-regulator hot loops compact.
4. Avoid routing temperature-sensitive references and precision sensor traces through concentrated thermal gradients.
5. Place high-loss regulators close enough to their intended thermal path to avoid long, resistive PCB heat paths.
6. Maintain mechanical keep-outs required by the SoM thermal interface and chassis hardware.
7. Define a thermal datum corresponding to the mechanical chassis datum used by the heat spreader.

## 10. SoM Thermal Interface

The selected DART-MX95 / i.MX 95 architecture remains the baseline compute target from ED-01. Exact SoM heat output shall be derived from the final module configuration and the applicable vendor power characterization rather than from the preliminary 8–12 W allocation alone.

The carrier shall therefore reserve:

- a central compute thermal interface region
- direct mechanical coupling to the aluminum spine
- clearance for the selected SoM module and heat spreader
- service access without disturbing adjacent RF or connector structures

No production enclosure temperature or junction-temperature claim is frozen by ED-05 until the exact SoM SKU/configuration is characterized.

## 11. Regulator Thermal Loss

ED-04 requires regulator thermal dissipation to be included in the ED-05 model.

For each regulator, record:

- input voltage
- output voltage
- continuous output current
- peak output current
- efficiency at nominal load
- efficiency at peak/mission load
- switching frequency
- package thermal resistance or validated board-level thermal data
- estimated dissipation
- PCB copper area used for heat spreading
- mechanical chassis thermal coupling, if any

The preliminary regulator sizing rule remains **≥125% of calculated continuous load**. Thermal design shall additionally verify that the selected regulator can sustain the required load at the worst permitted ambient temperature without violating its applicable thermal limits.

## 12. Ambient and Temperature Cases

Thermal qualification shall evaluate at least:

| Case | Purpose |
|---|---|
| Room-temperature nominal | Bring-up and correlation |
| Maximum specified ambient | Continuous thermal limit |
| Minimum specified ambient | Cold-start and thermal-gradient behavior |
| Sustained compute load | SoM thermal steady state |
| Sustained I/O/network load | Carrier/regulator heating |
| Compute + I/O combined | Representative mission worst case |
| Optional FPGA enabled | T3 variant screening |
| Enclosure-installed condition | Final system thermal correlation |

The final maximum ambient temperature is not frozen by ED-05; it shall be inherited from the applicable Nodem product variant environmental requirement.

## 13. Temperature Monitoring

The STM32H573 supervisory domain shall collect or supervise, where available:

- SoM reported temperature
- board temperature
- regulator hotspot temperature for critical converters
- FPGA temperature when populated
- battery-adjacent temperature sensor(s)
- thermal-fault/status signals from devices that provide them

Thermal telemetry shall be timestamped and included in the Nodem-SBC-OS health record.

Thermal protection shall support staged response:

```text
NORMAL
  |
  v
THERMAL ADVISORY
  |
  v
REDUCE / SHED NONESSENTIAL LOADS
  |
  v
THERMAL LIMIT
  |
  v
CONTROLLED SHUTDOWN / DOMAIN ISOLATION
```

Exact thresholds remain device- and variant-specific until component P/Ns and environmental requirements are frozen.

## 14. Mechanical Thermal Interface Closure

The Nodem mechanical architecture shall reserve the following interfaces for NOD-SBC-001:

- central thermal datum on the SBC
- copper spreader contact region
- aluminum spine contact region
- side heat-rail attachment points
- controlled fastener locations
- TIM service/replacement access
- battery thermal exclusion zone
- airflow/vent path where passive convection is used

The 160 × 100 mm PCB envelope remains the provisional reference envelope. ED-05 does not authorize a production dimensional freeze until the selected SoM, connector placement, thermal hardware, and mechanical interference checks are complete.

## 15. Thermal Design Margin

Thermal design shall not be closed using nominal room-temperature measurements alone.

Required margin methodology:

1. Model worst-case continuous electrical load.
2. Include conversion losses and local component losses.
3. Apply the worst permitted ambient condition.
4. Include enclosure/chassis thermal resistance.
5. Verify component-specific temperature limits from authoritative vendor data.
6. Reserve engineering margin between predicted operating temperature and the applicable component/system limit.
7. Correlate the model against instrumented prototype measurements.

A numerical production margin is intentionally **TBD** until authoritative limits for the selected SoM, regulators, FPGA, storage, and enclosure materials are assembled.

## 16. Verification Plan

ED-05 thermal verification shall include:

- infrared or equivalent surface-temperature mapping
- thermocouple/RTD measurements at critical hotspots
- SoM software thermal telemetry capture
- regulator hotspot measurement
- FPGA junction/temperature telemetry when populated
- steady-state soak testing
- load-step testing
- enclosure-installed testing
- worst-case ambient testing
- thermal-cycle testing as required by the product environmental requirement

The thermal model shall be correlated against measured temperatures before release qualification.

## 17. ED-05 Exit Criteria

ED-05 is considered **architecture-closed** when:

- the primary conductive thermal path is defined
- dominant heat sources have allocated thermal paths
- T1/T2/T3 classification is retained and applied
- regulator losses are explicitly included in the model
- PCB thermal-spreading requirements are defined
- SoM thermal interface geometry is reserved
- battery thermal exclusion is defined
- thermal telemetry requirements are assigned
- prototype thermal verification is specified

The following remain open for later implementation/qualification gates:

- exact thermal resistance values
- final TIM P/N and thickness
- final copper spreader geometry
- final chassis fin geometry
- exact SoM/FPGA thermal limits
- final regulator P/N losses
- worst-case ambient requirement per product variant
- prototype correlation data

## 18. Gate Status

**ED-05 disposition: THERMAL ARCHITECTURE ACCEPTED FOR MECHANICAL/ELECTRICAL DETAIL DESIGN.**

This closes the thermal architecture increment without claiming production thermal qualification.

## 19. Next Gate

**ED-06 — PCB envelope, connector allocation, and mechanical/electrical interface definition.**

ED-06 shall convert the provisional 160 × 100 mm board target and thermal/mechanical constraints into an implementable board envelope, mounting pattern, connector keep-outs, thermal interface datum, and preliminary pin-level interface allocation.
