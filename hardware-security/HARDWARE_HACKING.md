---
title: "Hardware Security: The Device Attack Surface"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: The Device Attack Surface

*Debug ports, buses, firmware, radios. The surfaces below the OS — where the device trusts whoever holds it.*

---

## Threat model the device first

Hardware security starts with the same question as software — what
could go wrong — but the assumptions differ:

| Software assumption | Hardware reality |
|---------------------|------------------|
| The attacker is remote | The attacker **owns the device**: opens it, probes it, reads its memory |
| The OS is the boundary | The chip is the boundary — and its debug ports are the doors |

Model the device as the attacker sees it: case off, probes on, no OS
in the way.

---

## The debug ports — UART, JTAG, SWD

| Port | What it is | What it gives the attacker |
|------|------------|---------------------------|
| **UART** | Serial console, often left enabled in production | A root shell, boot logs, secrets printed at boot |
| **JTAG** | Chip-level debug, boundary scan | Halt the CPU, read/write memory, dump firmware |
| **SWD** | ARM's two-wire JTAG equivalent | The same, on ARM |

The test: probe the board's headers and test points for a serial
console at common baud rates. **A device that ships its debug console
open is a device that shipped its root access.**

Defense: disable or fuse off debug interfaces in production, and log
what the boot process prints — those logs are the first thing a UART
leaks.

---

## The buses — SPI and I2C

Between the main chip and its peripherals — flash, sensors, secure
elements — data moves on serial buses. An attacker with a logic
analyser clips onto the traces and **watches the secrets cross the
wire**: flash contents during boot, sensor data, keys in transit.

The finding that matters: what travels **unencrypted between
components on the same board**. The defense: encrypted channels or
physically protected traces where the data is sensitive — and knowing
what is exposed either way.

---

## Firmware hacking

The firmware image is the device's soul — and usually its dump:

- **Acquisition**: from the vendor's update file, or read off the
  flash via the debug ports above.
- **Analysis**: filesystem extraction, strings, embedded keys and
  certificates, leftover debug binaries.
- **The classic findings**: hardcoded credentials, private keys,
  update mechanisms that accept unsigned images.

The defense is symmetric: **no secrets in firmware**, signed updates
enforced, and strip the debug build before shipping.

---

## Short-range radio — RFID and friends

Radio adds an attack surface that needs no physical contact: RFID
cards and tags trust proximity. The attacks are cloning (read the
tag, write another), relay (extend the read range), and spoofing —
and the defense questions are the same as everywhere: what does the
tag actually prove, and is that enough for what it unlocks?

---

## Rules of thumb

- **Threat model with the case off.** The attacker is not remote; the
  device is in their hands.
- **Check the debug ports before you ship** — and check them again in
  the production units, not the prototypes.
- **Watch the buses**: anything crossing SPI/I2C unencrypted is
  readable by a $20 logic analyser.
- **Treat firmware like source code**: reviewed, signed, stripped of
  debug, and free of secrets.

## Why it matters

The mesh's fleet runs on physical boxes, and the mesh's future
includes devices at the edge — weather stations, sensors, whatever
earns a place on a station. Hardware security is the discipline that
asks the device questions *before* the device ships: what does it
trust, what does it leak, and who can hold it while it answers.
