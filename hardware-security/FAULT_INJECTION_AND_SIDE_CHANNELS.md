---
title: "Hardware Security: Fault Injection and Side Channels"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: Fault Injection and Side Channels

*Correct code on real silicon still leaks through physics, and silicon under physical stress stops executing correct code. Devices that hold secrets must be designed for both.*

---

## What it is

Two families of physical attack target the chip rather than the
software's logic:

| Family | Principle | Typical impact |
|--------|-----------|----------------|
| **Side-channel analysis** | Power consumption, electromagnetic emission and timing depend on the data being processed | Recovery of cryptographic keys or other secrets |
| **Fault injection** | Disturbing supply voltage, clock, electromagnetic field, or light pushes the chip outside its operating envelope so instructions are skipped or data corrupted | A security check that fails open, or corrupted crypto output that leaks key material |

Both assume physical access or close proximity, so they matter most for
secure elements, smart cards, hardware wallets, payment terminals,
automotive ECUs and any device whose owner is not the party it protects.

## How it works, at concept level

**Side channels.** A transistor switching costs energy, and how many
switch depends on the values involved. *Simple* analysis reads
operations directly from one or a few traces (for example, key-dependent
branches). *Differential* and *correlation* analysis, introduced
publicly by Kocher, Jaffe and Jun in 1999, apply statistics over many
traces to test hypotheses about small parts of a key, so even noisy
leakage becomes exploitable. Timing channels work the same way over
execution time and can be remote.

**Faults.** Logic only works within its voltage, clock and temperature
margins. A brief disturbance at a sensitive moment can corrupt one
instruction or value. The classic concern is a single conditional
branch guarding something important (signature verification, a debug
lock, a PIN counter): if one fault can flip its outcome, the design has
a single point of failure. Faults during cryptographic computation can
also produce wrong outputs that reveal key material.

## Detection

- **On-chip sensors:** voltage, clock-frequency, temperature, light and
  EM glitch detectors that trigger a reset or key wipe.
- **Consistency checks:** redundant computation or verify-after-sign
  catches corrupted results before they leave the device.
- **Tamper response and logging:** count detector events and failed
  checks; a device seeing repeated anomalies should lock or erase
  secrets.
- **Leakage assessment during development:** statistical tests such as
  TVLA, and certification testing under FIPS 140-3 (non-invasive
  attacks) or Common Criteria, measure leakage before shipping.

## Prevention and mitigation

| Threat | Countermeasures |
|--------|-----------------|
| Timing and simple power analysis | Constant-time code with no secret-dependent branches or memory access |
| Differential / correlation analysis | Masking (split secrets into random shares), shuffling, hiding in noise, limiting key use per device |
| Voltage and clock faults | Detectors, internal clock sources, filtered supply |
| EM and optical faults | Shielding, sensors, dense layout, active meshes |
| Faults on security decisions | Redundant and inverted checks, random delays, hardened status encodings instead of single-bit flags, fail-closed defaults |

Mitigation is a co-design of silicon, firmware and protocol: a masked
algorithm on an unhardened chip, or a hardened chip running a
leaky library, is not protected.

## Trade-offs and pitfalls

- Countermeasures cost area, power and speed; masking multiplies
  computation.
- Compiler optimisation can silently remove redundancy or break
  constant-time properties; verify the compiled binary.
- A device secure against one trace or one fault may still fail against
  higher-order analysis or multiple faults; security levels are stated
  against a defined attacker budget.

## Related notes

[HARDWARE_HACKING](HARDWARE_HACKING.md) covers the board-level surface
around the chip; [CAR_HACKING](CAR_HACKING.md) applies secure-boot and
key-storage needs to vehicles.

## Sources

- *The Hardware Hacking Handbook: Breaking Embedded Security with Hardware Attacks*, Jasper van Woudenberg and Colin O'Flynn, No Starch Press, 2021. [Publisher page](https://nostarch.com/hardwarehacking)
- Paul Kocher, Joshua Jaffe, Benjamin Jun, "Differential Power Analysis", *Advances in Cryptology: CRYPTO '99*, LNCS 1666, Springer, 1999. [Springer](https://link.springer.com/chapter/10.1007/3-540-48405-1_25)
- NIST FIPS 140-3, *Security Requirements for Cryptographic Modules*, 2019. [CSRC](https://csrc.nist.gov/pubs/fips/140-3/final)
