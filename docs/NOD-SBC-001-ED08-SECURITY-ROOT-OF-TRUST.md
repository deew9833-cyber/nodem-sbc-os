# NOD-SBC-001 ED-08 — Security / Root-of-Trust Electrical Architecture

**Status:** Architecture baseline — implementation selection pending

## 1. Objective

Define the hardware security boundary for NOD-SBC-001 without prematurely freezing a specific secure-element or processor part number.

## 2. Trust hierarchy

```text
Hardware Root of Trust
        |
        +-- Device Identity / Hardware Key Material
        |
        +-- Secure Boot Anchor
        |
        +-- Boot Measurement
        |
        +-- Platform Attestation
        |
        +-- Firmware / OS Policy
        |
        +-- Mission Application
```

## 3. Security domains

### SoM / Application processor

- authenticated boot chain
- measured boot
- trusted execution/security monitor where supported
- protected key access
- lifecycle state enforcement

### Security controller / secure element

Provide an independent hardware trust boundary for device identity, key protection, attestation credentials, monotonic state/counters, and cryptographic operations where required.

### STM32H573 supervisory domain

- secure management firmware
- power/security state supervision
- recovery authorization
- watchdog and fault response
- protected service interface

### FPGA domain

If fitted, FPGA configuration shall be authenticated and bound to the approved platform configuration. FPGA is not permitted to establish an independent root of trust above the system security policy.

## 4. Key hierarchy

```text
Device Root Identity
        |
        +-- Attestation Identity
        |
        +-- Firmware Verification Keys
        |
        +-- Platform / Session Derivation
        |
        +-- Mission Credentials
```

Private root material shall not be exposed through ordinary CPU, OS, debug, or expansion interfaces.

## 5. Debug policy

JTAG/UART service access is treated as a security boundary. Production configuration shall support authenticated debug authorization and irreversible/controlled debug lockdown consistent with the selected silicon lifecycle mechanism.

## 6. PQC boundary

Post-quantum cryptography shall be exposed through a cryptographic abstraction layer. NOD-SBC-001 hardware shall support algorithm agility and hardware acceleration where available, but application software shall not depend on a single hard-wired PQC implementation.

## 7. Secure storage

Security-sensitive material shall use hardware-protected storage or a dedicated secure element. NVMe/eMMC are mission storage devices and are not, by themselves, considered the root key store.

## 8. Security interfaces

- Secure boot: SoM security controller / ROM chain
- Identity: secure element or SoM hardware security facility
- Attestation: secure element + measured platform state
- Management: STM32H573 protected supervisory channel
- FPGA authentication: authenticated configuration image
- Debug: controlled JTAG/UART lifecycle state
- Storage: encrypted/authenticated mission-data path as required by system policy

## 9. Verification requirements

ED-08 verification shall include:

1. secure-boot negative tests
2. unauthorized firmware rejection
3. measured-boot integrity verification
4. device-identity uniqueness test
5. attestation verification
6. protected-key non-export test
7. debug-lock policy test
8. rollback/lifecycle-state test
9. FPGA-image authentication test when fitted
10. PQC cryptographic self-test and interoperability testing

## 10. Freeze boundary

The following remain open pending silicon/package/pinmux review:

- exact secure-element P/N
- exact SoM security feature configuration
- secure-element bus assignment
- security GPIO/reset topology
- production debug-lock mechanism
- key-injection manufacturing process
- final PQC accelerator selection

**ED-08 disposition: SECURITY ARCHITECTURE ACCEPTED — COMPONENT/PIN-LEVEL IMPLEMENTATION PENDING.**
