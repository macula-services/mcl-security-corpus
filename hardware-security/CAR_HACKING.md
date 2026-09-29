---
title: "Hardware Security: Vehicle Hacking"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: Vehicle Hacking

*The car is a network of ECUs on buses, trust-free by design. The attack surface is the bus, the diagnostic port, and every radio.*

---

## The model

A vehicle is **dozens of ECUs** (engine, brakes, infotainment…)
talking over **buses** — primarily CAN — with no authentication
between them: a message is trusted if it is *well-formed*, not if it
is *authorised*. The bus was designed when the only connection was a
wrench; now the vehicle has radios.

The threat model follows: **anything that can reach the bus can speak
for any ECU.**

---

## The attack surface

| Surface | What it is | The attack |
|---------|------------|------------|
| **CAN bus** | The vehicle's main nervous system, broadcast, no source addresses | Inject frames — brake, throttle, steering messages |
| **OBD-II port** | The mandated diagnostic connector — physical access to the bus | The standard entry: plug in, speak CAN |
| **Diagnostics** | UDS requests over CAN — read/write ECU state | Reprogram ECUs, disable functions |
| **Infotainment (IVI)** | A networked computer with a radio, bridged to vehicle buses | Remote entry: exploit the IVI, pivot to CAN |
| **V2V / telematics** | Wireless channels to and from the vehicle | Remote attacks without physical access |
| **Keyless entry, tire sensors** | Short-range radio protocols | Replay, relay, spoof |

The hierarchy of access: physical OBD is the strongest position;
IVI exploitation is the path that makes it remote.

---

## The workflow

1. **Threat model the vehicle** — which ECUs, which buses, what does
   the attacker want (theft, disablement, data)?
2. **Connect** — OBD-II or a bench harness; `SocketCAN` on Linux
   gives the bus as a network interface.
3. **Observe** — log the traffic; reverse-engineer which frame does
   what (the hard part: correlating frames with behaviour).
4. **Isolate** — a bench with one ECU lets you experiment safely,
   away from the vehicle.
5. **Inject** — send crafted frames; observe the effect. The finding
   is *control*: "frame X, sent once, unlocks the doors."
6. **Radio** — SDR for the wireless protocols: keyless entry,
   TPMS, V2V.

---

## Rules of thumb

- **Never test on a vehicle in motion, ever.** Bench first, then
  stationary, with the safety systems understood. The stakes are
  physical.
- **The bus trusts form, not sender.** Every defense must be
  layered *above* the bus — message authentication, gateway
  firewalls — because the bus itself will never be retrofitted.
- **Correlate before injecting.** One frame at a time, observed,
  understood — the difference between a finding and a bricked ECU.
- **The IVI is the bridge.** Segment it from the vehicle buses; a
  hardened IVI makes the remote path die at the head unit.

## Why it matters

The same pattern as every embedded system, at vehicle scale: legacy
buses, assumed trust, radios added later. Vehicle hacking is the
clearest case study of the hardware-security rule — **threat model
with the case open, because the attacker will be holding the wrench
(or the radio).**
