# NOD-SBC-001 — ED-02 FPGA/MCU Selection

**Baseline:** Rev A electrical-design increment  
**Status:** Recommended architecture; production P/N freeze pending schematic and SI/PI closure

## 1. Decision

Use a **two-tier deterministic-control architecture**:

- **Primary MCU:** STM32H573-class Cortex-M33 controller for supervisory control, secure management, power sequencing, health monitoring, watchdog, service interfaces, and deterministic low-rate I/O.
- **Optional FPGA:** Lattice Avant-E, initially E30/E50 class, for variants requiring deterministic high-throughput I/O, protocol bridging, timing, packet/data-path acceleration, or custom hardware acceleration.

The baseline NOD-SBC-001 configuration therefore does **not** make a large FPGA mandatory. This reduces Rev-A power, thermal, PCB, and software complexity while preserving an FPGA expansion path.

ST documents the STM32H573 as a 250 MHz Cortex-M33 with TrustZone, up to 2 MB Flash and 640 KB RAM, ECC-backed memory features, and hardware cryptographic acceleration. citeturn0search0turn0search4

Lattice's current Avant-E family provides industrial/automotive/defense temperature options, substantial logic/DSP resources, LPDDR4/DDR4 interfaces, and a low-power positioning appropriate to an optional deterministic fabric. citeturn0search2turn0search5

## 2. Allocation

```text
NXP i.MX 95 SoM
       │
       ├── Linux / applications / networking / AI
       │
       ├── PCIe / USB / Ethernet
       │
       └── management bus
                 │
                 ▼
          STM32H573 MCU
          ┌──────┼────────┐
          │      │        │
       Power   Health   Service
       Control Monitor  / I/O
          │      │        │
          └──────┼────────┘
                 │
          Optional FPGA
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
    Timing    Data Path   Protocol
    / I/O     Acceleration Bridging
```

## 3. Candidate Trade

| Candidate | Role | Strength | NOD-SBC-001 disposition |
|---|---|---|---|
| STM32H573 | Supervisory MCU | Security, low power, deterministic control | **Primary** |
| Lattice Avant-E E30/E50 | FPGA fabric | Large programmable fabric with low-power orientation | **Optional primary FPGA** |
| AMD Artix UltraScale+ | High-performance FPGA | High-speed transceivers and large FPGA resources | High-performance variant |
| FPGA-only architecture | Deterministic fabric | Maximum flexibility | Rejected for Rev A baseline |

AMD Artix UltraScale+ provides considerably more transceiver and FPGA capacity than the minimum Rev-A requirement, making it attractive for high-throughput RF/data-path variants but excessive for the common low-power carrier. citeturn0search3

## 4. Architectural Rationale

The i.MX 95 remains the application processor selected in ED-01. The STM32H573 supplies an independent management/control plane. An FPGA is added only where requirements demonstrate a need for programmable deterministic logic or high-rate hardware data paths.

This prevents the Rev-A board from carrying an unnecessary FPGA power/thermal burden while keeping the Nodem platform extensible.

## 5. Interface Ownership

### STM32H573

- power-management control
- PMIC sequencing
- board health telemetry
- thermal sensors
- fan/pump control where applicable
- watchdog supervision
- service UART
- low-speed GPIO
- CAN-FD where required
- secure management functions
- recovery/control path

### Optional FPGA

- deterministic high-speed I/O
- custom packet/data path
- timing distribution
- protocol conversion
- RF/sensor preprocessing
- hardware acceleration
- high-rate capture/generation

### i.MX 95

- application processing
- Linux/Nodem-SBC-OS
- networking
- storage
- AI/NPU workloads
- mission applications

## 6. Gate Status

**ED-02 disposition: RECOMMENDED.**

Before production component lock, verify:

1. STM32H573 package/pinmux against connector requirements.
2. Required MCU peripherals and DMA channels.
3. FPGA I/O and transceiver requirements for each mission variant.
4. Power consumption at representative workloads.
5. Thermal contribution to the aluminum spine budget.
6. Secure-boot/control-plane trust relationships.
7. Clock/reset architecture.
8. PCB escape and SI/PI feasibility.

## 7. Next Gate

Proceed to **ED-03 — quantitative power budget and power-tree design** using:

- i.MX 95 SoM as application-domain baseline
- STM32H573 as mandatory management-domain baseline
- FPGA as an optional/high-performance population
- NOD-SBC-001 160 × 100 mm PCB target retained pending electrical closure

No production FPGA P/N is frozen by this document.
