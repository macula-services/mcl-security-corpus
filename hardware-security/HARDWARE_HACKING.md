---
title: "Hardware Security: The Device Attack Surface"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: The Device Attack Surface

*Once someone holds the device, the operating system is no longer the boundary. Debug interfaces, board-level buses, firmware and radios are.*

---

## What it is

Hardware security asks what a device still protects when an adversary
physically possesses it. For embedded and IoT products this is the
normal case: devices are sold, lost, discarded and resold. Threat
modelling therefore has to include the device on a bench, not only the
device on a network.

| Network-centric assumption | Physical-possession reality |
|----------------------------|-----------------------------|
| The adversary is remote | The adversary can open the enclosure |
| The OS enforces isolation | Components below the OS are reachable directly |
| Secrets at rest are safe | Storage parts can be examined outside the device |

## The four exposure areas

- **Debug interfaces.** Manufacturing and repair ports (serial consoles,
  chip debug interfaces such as JTAG and SWD) are useful in the factory
  and a liability in the field if left enabled.
- **Board-level buses.** Links between processor, flash, sensors and
  secure elements are usually unencrypted, so sensitive data crossing
  them is exposed to anyone with physical access.
- **Firmware.** The image carries code, configuration and, in weak
  designs, secrets shared by every unit, leftover debug features, and an
  update path that accepts unverified images.
- **Short-range radio.** RFID, NFC, BLE and remote-control protocols
  that rely on a static identifier prove little; the design question is
  what the exchange actually authenticates.

## Detection and assessment

- Review **production** units, not prototypes: debug settings often
  differ between the two.
- Inventory what each external or internal interface exposes, and what
  data crosses each bus in plaintext.
- Examine firmware you ship (or are authorised to assess) for embedded
  credentials, keys, test tooling, and unsigned update handling, using
  the [reversing workflow](../reversing/REVERSE_ENGINEERING.md).
- Watch fleet telemetry for signs of tampering: unexpected firmware
  versions, failed signature checks, enclosure-open sensors.

## Prevention and mitigation

| Area | Controls |
|------|----------|
| Debug | Disable or permanently lock debug in production; authenticated debug where field service needs it |
| Buses | Keep secrets inside a secure element; encrypt or authenticate sensitive inter-chip traffic |
| Firmware | Secure boot with a hardware root of trust; signed updates with rollback protection; per-device credentials; no secrets in images |
| Radio | Challenge-response with per-device keys; distance bounding where relays matter |
| Lifecycle | Secure decommissioning that wipes keys; vulnerability disclosure and update support for the product's life |

NIST SP 800-193 frames the firmware goals as protect, detect and
recover, which is a useful checklist for any device design.

## Trade-offs and pitfalls

- Locking debug complicates repair and failure analysis; plan an
  authenticated service path instead of leaving the port open.
- A single key shared across a product line turns one compromised unit
  into a fleet-wide compromise.
- Signed updates without rollback protection still allow a downgrade to
  a vulnerable version.

## Related notes

[FAULT_INJECTION_AND_SIDE_CHANNELS](FAULT_INJECTION_AND_SIDE_CHANNELS.md)
covers attacks on the chip itself; [CAR_HACKING](CAR_HACKING.md) applies
the same model to vehicles.

## Sources

- *Practical IoT Hacking: The Definitive Guide to Attacking the Internet of Things*, Fotios Chantzis, Ioannis Stais, Paulino Calderon, Evangelos Deirmentzoglou, Beau Woods, No Starch Press, 2021. [Publisher page](https://nostarch.com/practical-iot-hacking)
- *The Hardware Hacker: Adventures in Making and Breaking Hardware*, Andrew "bunnie" Huang, No Starch Press, 2017 (paperback 2019). [Publisher page](https://nostarch.com/hardwarehackerpaperback)
- NIST SP 800-193, *Platform Firmware Resiliency Guidelines*, 2018. [CSRC](https://csrc.nist.gov/pubs/sp/800/193/final)
- NIST IR 8259A, *IoT Device Cybersecurity Capability Core Baseline*, 2020. [CSRC](https://csrc.nist.gov/pubs/ir/8259/a/final)
- OWASP Internet of Things Project. [OWASP](https://owasp.org/www-project-internet-of-things/)
- MITRE ATT&CK, Pre-OS Boot (T1542). [MITRE](https://attack.mitre.org/techniques/T1542/)
