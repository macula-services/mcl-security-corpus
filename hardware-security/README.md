---
title: Hardware Security
layer: index
audience: [agent, human]
stage: stable
---

# Hardware Security

*The attack surface below the OS: debug ports, buses, firmware, radios.*

---

## Notes

| Note | Covers |
|------|--------|
| [HARDWARE_HACKING](HARDWARE_HACKING.md) | Threat modeling the device, UART/JTAG/SWD, SPI/I2C, firmware, radio |

## One-page summary

Every device has an attack surface the operating system never sees:
**debug ports** (UART, JTAG, SWD) that speak the chip's language,
**buses** (SPI, I2C) where secrets travel between components,
**firmware** that ships with debug leftovers, and **radios** (RFID,
BLE) that trust proximity. Hardware hacking is the practice of finding
those surfaces — and the defense is knowing they exist before shipping.
