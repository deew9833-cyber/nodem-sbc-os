# NOD-SBC-OS CONTENT PROVENANCE 001

## Control State

- Repository: `deew9833-cyber/nodem-sbc-os`
- Branch: `main`
- Inventory basis: recursive Git tree inspection
- Inventory result: 21 files / 1 directory tree
- Repository visibility: PUBLIC (existing repository metadata; privacy change not performed)
- Closure state: NOT CLOSED

## Evidence Boundary

This manifest records the Git object identifiers and file sizes returned by the GitHub recursive tree inspection. The listed blob SHA values are Git object identifiers and MUST NOT be represented as SHA-256 content hashes.

A cryptographic SHA-256 content inventory is therefore `TBD` pending byte-level capture of each file. No SHA-256 value is fabricated here.

## Current Tree Inventory

| Path | Type | Size (bytes) | Git blob SHA | SHA-256 |
|---|---|---:|---|---|
| `README.md` | blob | 793 | `60d908aa2a76183a47b876c429160fcea2634597` | TBD |
| `docs/NOD-SBC-001-ED01-CPU-SOM-TRADE-STUDY.md` | blob | 10738 | `aa2d71ff8c3d917b9883063ec764e201906f5cce` | TBD |
| `docs/NOD-SBC-001-ED02-FPGA-MCU-SELECTION.md` | blob | 4837 | `f41c31cc594affac10aa38035eab77f7c5e53d70` | TBD |
| `docs/NOD-SBC-001-ED03-POWER-BUDGET-TREE.md` | blob | 12701 | `28607427abcdc88def37540590e2fd724ccf9dbb` | TBD |
| `docs/NOD-SBC-001-ED04-POWER-COMPONENT-SELECTION.md` | blob | 3909 | `c605aac422a29669795af2c8dae6aee2746891bc` | TBD |
| `docs/NOD-SBC-001-ED05-THERMAL-BUDGET-STACK.md` | blob | 14746 | `a0fef5a47a7176ab7437eddaf407b624faa772af` | TBD |
| `docs/NOD-SBC-001-ED06-MECHANICAL-CONNECTOR-INTERFACE.md` | blob | 6857 | `e707647135a92ed0c8517e40ae5cbdf607e7c46f` | TBD |
| `docs/NOD-SBC-001-ED07-HIGH-SPEED-INTERFACE-ALLOCATION.md` | blob | 10952 | `59725d0c03e6016a3617692f0d46383e408a9067` | TBD |
| `docs/NOD-SBC-001-ED08-SECURITY-ROOT-OF-TRUST.md` | blob | 3866 | `3eb2fe1e02975df5093d6ef1d6325e56a9936ecb` | TBD |
| `docs/NOD-SBC-001-ED09-KICAD-SCHEMATIC-SCAFFOLD.md` | blob | 4271 | `7bb677ebdc0c6aaf0505ce09916fb796cc0c46ec` | TBD |
| `docs/NOD-SBC-001-ED10-BOM-LIFECYCLE-AVAILABILITY.md` | blob | 9617 | `664bb7399c7574543030330df22c809c204ddadb` | TBD |
| `docs/NOD-SBC-001-ED11-EXACT-PART-SELECTION.md` | blob | 4802 | `1b82d89c74b9e343e3f2b6a693ed6d2073dc114f` | TBD |
| `docs/NOD-SBC-001-ED12-VERIFICATION-REQUIREMENTS.md` | blob | 8810 | `abc91732cdbb76a9486dd0d6dcbd59bce0a061f9` | TBD |
| `docs/NOD-SBC-001-ED13-BRINGUP-VERIFICATION-PROCEDURES.md` | blob | 9934 | `1c3242bab4616d81bc7b5664d245efa6aff5a83c` | TBD |
| `docs/NOD-SBC-001-ED14-TEST-RECORDS-ACCEPTANCE-LIMITS.md` | blob | 14821 | `066acd89843b8b5354354423120ad85ce6890cf2` | TBD |
| `docs/NOD-SBC-001-ED15-ACCEPTANCE-LIMITS-TEST-PACKAGE.md` | blob | 14903 | `03b856ccb5ca0ff8423a2caa985d0637aa44dfde` | TBD |
| `docs/NOD-SBC-001-ED16-PROTOTYPE-CONFIGURATION-READINESS-REVIEW.md` | blob | 19576 | `b692fa2cd9e4bab11cb594faf0578486a43dfbf1` | TBD |
| `docs/NOD-SBC-001-ED17-FIRST-ENERGIZATION-SESSION.md` | blob | 8791 | `e303f8c983740925419b6d1f125a6fafc7cd5dc5` | TBD |
| `docs/NOD-SBC-001-ED18-POWER-RAIL-MCU-BRINGUP.md` | blob | 3769 | `f158b911cbb1a388f48cd362c8ea6cb0ba07c452` | TBD |
| `docs/NOD-SBC-001-ELECTRICAL-DESIGN-INCREMENT.md` | blob | 6336 | `f79e6d280090f8ce18cc1094e236207b573107a2` | TBD |
| `docs/NOD-SBC-001-REFERENCE-DESIGN.md` | blob | 8673 | `6f286c6bfc256803179b6ea35456567c3d5c5d78` | TBD |

## Controlled Disposition

- Repository identity: EVIDENCED
- Current branch: EVIDENCED — `main`
- Content inventory: EVIDENCED — 21 files
- Git object identifiers: EVIDENCED
- SHA-256 content inventory: OPEN / TBD
- Independent byte-level capture: OPEN / TBD
- Provenance completeness: OPEN
- Privacy disposition: OPEN — repository remains public
- Baseline closure: NOT CLAIMED

## Required Next Evidence

1. Capture file bytes for all 21 text files from the controlled `main` revision.
2. Compute SHA-256 over the exact captured bytes.
3. Reconcile the resulting hashes and byte counts against the Git tree.
4. Preserve the resulting manifest as the cryptographic content baseline.
5. Separately resolve repository visibility through a GitHub administration capability; visibility is not a commit property.
