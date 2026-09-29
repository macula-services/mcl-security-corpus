---
title: "Hardware Security: Vehicle Hacking"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: Vehicle Hacking

*A modern vehicle is a distributed system of ECUs on shared buses that were designed for a closed network. Connectivity opened that network; defence now has to be added above the bus.*

---

## What it is

A car contains dozens of electronic control units (ECUs) for engine,
braking, body functions, infotainment and telematics. Most talk over
CAN (Controller Area Network), alongside LIN, FlexRay and automotive
Ethernet. Classic CAN has **no sender authentication**: a frame carries
an identifier that describes the message type and its bus priority, not
who sent it, and any node on the bus can transmit any identifier.
Receivers act on well-formed frames. That was reasonable when the bus
was physically closed; it is not now that vehicles carry cellular,
Wi-Fi and Bluetooth links.

## The attack surface

| Entry point | Why it matters |
|-------------|----------------|
| **OBD-II diagnostic port** | Mandated diagnostic access to in-vehicle networks; physical access to the car means access to a bus |
| **Diagnostic services (UDS, ISO 14229)** | Powerful functions (reprogramming, routine control) gated by "security access" schemes that are sometimes weak |
| **Infotainment and telematics** | Networked computers with radios; if bridged to safety-relevant buses, a remote compromise can become a vehicle-control problem |
| **Short-range radio** | Keyless entry and tyre-pressure sensors; relay attacks on passive keyless entry are a known theft method |
| **Supply chain and aftermarket devices** | Dongles plugged into OBD-II become permanent, network-connected bus nodes |

The attack class of concern is **unauthorised message injection or
impersonation**: a compromised or rogue node sending frames that
receiving ECUs treat as genuine.

## Detection

- **CAN intrusion detection.** Most CAN traffic is periodic, so
  deviations in message timing and frequency, unknown identifiers, or
  out-of-range signal values are strong anomaly signals.
- **Gateway monitoring.** A central gateway sees cross-domain traffic
  and can log and alert on requests that should never cross (for
  example, diagnostic sessions opened from the infotainment domain while
  driving).
- **Diagnostic session logging.** Record security-access attempts and
  reprogramming requests; repeated failures indicate probing.
- **Fleet-level monitoring.** UNECE Regulation 155 requires
  manufacturers to run a certified cybersecurity management system that
  includes detecting and responding to attacks across their vehicles in
  service; many do this through a vehicle security operations centre.

## Prevention and mitigation

| Control | Effect |
|---------|--------|
| **Domain separation and gateway filtering** | Infotainment and telematics cannot send safety-relevant frames; only whitelisted identifiers cross domains |
| **Message authentication (AUTOSAR SecOC)** | A truncated MAC plus a freshness value on critical frames lets receivers reject forged or replayed messages |
| **Strong diagnostic authentication** | Replace weak seed/key schemes with certificate-based authorisation; lock diagnostics while moving |
| **Secure boot and signed updates** | ECUs run only manufacturer-signed firmware, including over-the-air updates |
| **Relay resistance** | Distance bounding (for example UWB ranging) for passive keyless entry |
| **Engineering process** | Threat analysis and risk assessment per ISO/SAE 21434 across the vehicle lifecycle |

## Trade-offs and pitfalls

- CAN frames are small (8 data bytes in classic CAN), so MACs are
  truncated and freshness must be managed carefully; CAN FD eases this.
- Retrofitting authentication onto existing platforms is slow; gateway
  segmentation is usually the first achievable control.
- Safety testing matters: security assessment belongs on a bench or a
  stationary, controlled vehicle, never on a public road.

## Related notes

The vehicle is a case of the general model in
[HARDWARE_HACKING](HARDWARE_HACKING.md). ECU firmware review uses the
[reversing workflow](../reversing/REVERSE_ENGINEERING.md).

## Sources

- *The Car Hacker's Handbook: A Guide for the Penetration Tester*, Craig Smith, No Starch Press, 2016. [Publisher page](https://nostarch.com/carhacking)
- AUTOSAR, *Specification of Secure Onboard Communication Protocol*, FO R24-11. [autosar.org (PDF)](https://www.autosar.org/fileadmin/standards/R24-11/FO/AUTOSAR_FO_PRS_SecOcProtocol.pdf)
- NHTSA, *Cybersecurity Best Practices for the Safety of Modern Vehicles*, 2022. [nhtsa.gov](https://www.nhtsa.gov/document/cybersecurity-best-practices-safety-modern-vehicles-2022)
- Linux kernel documentation, SocketCAN. [kernel.org](https://www.kernel.org/doc/html/latest/networking/can.html)
