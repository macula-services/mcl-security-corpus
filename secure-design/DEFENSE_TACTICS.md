---
title: "Secure Design: Defense Tactics"
layer: guide
audience: [agent, human]
stage: stable
---

# Secure Design: Defense Tactics

*Defense is a discipline, not a product: map what you defend, guard the few things that matter, deceive the rest, and design for the attacker already inside.*

---

## The castle model

The ninja manual's advice to defenders translates directly: a castle
that guards everything guards nothing. The tactical principles:

| Principle | Meaning |
|-----------|---------|
| **Map the network first** | You cannot defend what you have not drawn — the map is the first artifact |
| **Guard with special care** | Identify the few assets worth exceptional protection; concentrate effort there |
| **Xenophobic security** | Treat the unknown as hostile by default; allow, do not deny |
| **Deception** | The attacker's cost rises with every false target — honeypots, canaries, decoys |
| **Assume infiltration** | Design for the attacker already inside: segmentation, least privilege, monitoring |

---

## The tactical toolkit

| Tactic | What it looks like in practice |
|--------|-------------------------------|
| **Mapping** | Asset inventory, network diagrams, data-flow maps — continuously updated |
| **Double-sealing** | Two independent controls on the critical door — MFA is the password's second seal |
| **Hours of infiltration** | The attacker needs time; detection and response shrink the window — alert fast, contain fast |
| **Access to time** | Logs with trusted timestamps — attribution and reconstruction depend on time being honest |
| **Bridges and ladders** | Know your own pivot paths: which host reaches what — the attacker's lateral map is yours first |
| **Locks** | The classical controls — passwords, keys, ACLs — as the baseline, never the whole defense |
| **Moon on the water** | Deception: what the attacker sees is not what is — canary files, decoy services |

---

## Rules of thumb

- **The map is the first control.** Every other tactic operates on the
  map; a defense without one is patrols in the dark.
- **Concentrate, then deceive.** Guard the few assets that matter with
  everything; make everything else a cost for the attacker to touch.
- **Default-deny beats default-allow.** Every open door must justify
  itself; the unknown is hostile until proven otherwise.
- **Assume the perimeter is already crossed.** Segmentation, least
  privilege, and detection assume an attacker inside — which is the
  only assumption that survives reality.

## Why it matters

These tactics are the human layer of the other notes: where
[SECURE_DESIGN](SECURE_DESIGN.md) gives the process and
[NETWORK_MONITORING](../network-security/NETWORK_MONITORING.md) the
sensors, this note gives the posture — the few principles that decide
whether the machinery is pointed at the right targets.
