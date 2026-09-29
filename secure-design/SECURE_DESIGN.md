---
title: "Secure Design: Threat Modeling"
layer: guide
audience: [agent, human]
stage: stable
---

# Secure Design: Threat Modeling

*"What could possibly go wrong?" — asked unironically. Model the system, find the attack surfaces and trust boundaries, enumerate and rank threats, mitigate the top.*

---

## The four questions

The whole discipline in one breath (the Four Questions Framework):

1. **What are we working on?** — a model of the system.
2. **What can go wrong?** — threats against that model.
3. **What are we going to do about it?** — mitigations.
4. **Did we do a good job?** — verify the mitigations hold.

Security design is this loop, run early and repeatedly — not a review
bolted on before release.

---

## STRIDE — the threat categories

The mnemonic enumerates what an attacker might do to an asset:

| Letter | Threat | Question it answers |
|--------|--------|---------------------|
| S | **Spoofing** | Can someone pretend to be someone else? |
| T | **Tampering** | Can data be changed in transit or at rest? |
| R | **Repudiation** | Can an action be denied, with no proof? |
| I | **Information disclosure** | Can someone read what they should not? |
| D | **Denial of service** | Can the service be made unavailable? |
| E | **Elevation of privilege** | Can someone gain rights they were not granted? |

Walk the model component by component and ask all six questions of
each — the checklist that keeps threat enumeration from being a
creativity contest.

---

## The vocabulary that matters

| Term | Meaning |
|------|---------|
| **Asset** | Valuable data or resources that need protection |
| **Attack surface** | A place where an attack could originate — every input, interface, endpoint |
| **Trust boundary** | An interface bridging more-trusted parts with less-trusted parts — where validation must happen |
| **Threat** | A way an attacker could harm an asset |

Two of these drive design: **the trust boundary is where checks go**
(nothing crosses it unvalidated), and **the attack surface is what you
shrink** (every input you remove is an attack you removed).

---

## The process

1. Work from a **model** of the system — everything in scope, drawn.
2. Identify the **assets**.
3. Scour the model component by component: attack surfaces, trust
   boundaries, threats (STRIDE).
4. Analyse the threats, most concrete first.
5. **Rank** them, most to least critical.
6. Propose mitigations for the critical ones.
7. Apply mitigations, most impactful and easiest first, until
   diminishing returns.
8. **Test the mitigations**, starting with those for the most critical
   threats.

For complex systems a complete threat inventory is infeasible — that
is the point of ranking: spend the effort where the risk is, not where
the list is longest.

---

## Rules of thumb

- **Threat-model before code, not after.** The model shapes the
  boundaries; retrofit security discovers the boundaries were wrong.
- **Shrink the attack surface as a feature.** Every removed input,
  port, or endpoint is a threat class deleted, not mitigated.
- **Validate at trust boundaries, not at the edges.** Data entering
  the trusted side must be checked exactly once, at the crossing.
- **Mitigations must be testable.** A mitigation you cannot verify is
  a hope; the last step of the process exists to catch hopes.

## Why it matters

Every downstream security practice — pentests, monitoring, reversing
— assumes the system had a design to attack. Threat modeling is the
practice that makes "secure by design" a claim you can check instead
of a phrase in a pitch deck.
