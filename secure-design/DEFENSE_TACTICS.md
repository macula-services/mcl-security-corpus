---
title: "Secure Design: Defense Tactics"
layer: guide
audience: [agent, human]
stage: stable
---

# Secure Design: Defense Tactics

*Defense is a practice, not a purchase: know what you have, protect the few things that matter most, make intrusion expensive and noisy, and plan as if someone is already inside.*

---

## What this note covers

[SECURE_DESIGN](SECURE_DESIGN.md) is about shaping a system before it
exists. This note is about operating one: the standing posture a
defending team takes once systems are live and adversaries are real.

---

## Five working principles

| Principle | In practice |
|-----------|-------------|
| **Inventory first** | Maintain a current list of hosts, services, identities and data stores, plus how they connect. Controls you cannot map to an asset are guesses. |
| **Concentrate protection** | Identify the handful of crown-jewel assets (signing keys, identity provider, customer data) and give them controls the rest of the estate does not get. |
| **Default deny** | Unknown traffic, software and accounts are refused until explicitly allowed: allow-listed egress, application allow-listing, closed-by-default firewall rules. |
| **Deception** | Plant things no legitimate user touches, such as canary credentials, decoy shares or honeypot services, so that any interaction is a high-confidence alert. |
| **Assume breach** | Segment networks, grant least privilege, and monitor internal traffic, so that one compromised host does not mean a compromised organisation. |

---

## Concrete controls behind each principle

- **Inventory**: automated asset discovery, a CMDB that is reconciled
  against network scans, data-flow diagrams kept next to the code.
- **Layered authentication**: phishing-resistant MFA on anything that
  reaches the crown jewels, so a stolen password alone opens nothing.
- **Time as a defensive resource**: an intruder needs time to move from
  foothold to objective. Fast detection and a rehearsed containment
  playbook shorten that window. Accurate, synchronised timestamps on
  logs make reconstruction and attribution possible.
- **Know your own pivot paths**: map which hosts can reach which, and
  which credentials are cached where. An attacker plans lateral
  movement from exactly this map; build it before they do.
- **Baseline controls**: passwords, keys, ACLs and patching are
  necessary, never sufficient.

---

## How defenders and testers use it

- **Blue teams** turn the principles into measurable items: percent of
  assets inventoried, crown-jewel systems behind MFA, canaries deployed,
  segments with internal monitoring.
- **Testers and red teams** probe the assumptions: find an asset not on
  the inventory, a path around segmentation, a decoy that fails to
  alert. Each finding is a gap in one of the five principles.

## Trade-offs and pitfalls

- Default deny carries operational friction; without a fast, owned
  exception process, teams route around it.
- Deception only works if alerts from it are triaged immediately. An
  unwatched honeypot is inventory, not defense.
- Concentrating protection requires an honest ranking of assets;
  treating everything as critical is the same as treating nothing as
  critical.
- "Assume breach" is a design constraint, not a slogan: it needs
  segmentation and internal telemetry to mean anything.

## Related notes

[NETWORK_MONITORING](../network-security/NETWORK_MONITORING.md) supplies
the sensors that make deception and assume-breach observable;
[NETWORK_ATTACKS](../pentest/NETWORK_ATTACKS.md) describes the lateral
movement these tactics are meant to slow down.

## Sources

- *Cyberjutsu: Cybersecurity for the Modern Ninja*, Ben McCarty, No Starch Press, 2021. https://nostarch.com/cyberjutsu
- NIST SP 800-207, *Zero Trust Architecture*, S. Rose, O. Borchert, S. Mitchell, S. Connelly, 2020. https://csrc.nist.gov/pubs/sp/800/207/final
- CIS Critical Security Control 1: Inventory and Control of Enterprise Assets. https://www.cisecurity.org/controls/inventory-and-control-of-enterprise-assets
- MITRE Engage (adversary engagement and deception framework). https://engage.mitre.org/
