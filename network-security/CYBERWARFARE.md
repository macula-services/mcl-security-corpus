---
title: "Network Security: Cyberwarfare"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: Cyberwarfare

*Nation-state operations are the upper bound of the threat model: what happens when the attacker is patient, funded, and attributed to a state.*

---

## What state operations look like

| Class | Shape |
|-------|-------|
| **Nation-state attacks** | Espionage and sabotage: long dwell times, custom implants, supply-chain reach |
| **State-sponsored financial attacks** | The state's deniable hand in theft — laundering intent through crime |
| **Human-driven ransomware** | Operators at keyboards, not just malware — negotiation, pressure, and hands on the network |
| **Election hacking** | Influence and integrity attacks against the democratic process itself |

The defining property is **resources and patience**: the state attacker
can wait months, buy zero-days, and burn infrastructure — the
defender's usual advantages (time, cost asymmetry) invert.

---

## Adversaries and attribution

Attribution is the hardest problem in the field: it is *analysis*, not
forensics. The chain:

1. **TTPs** — tools, techniques, procedures: how the malware behaves,
   what infrastructure it uses, the operational patterns. These are
   the durable fingerprints.
2. **Correlation** — the same TTPs across campaigns; infrastructure
   overlap; linguistic and timezone artefacts.
3. **Confidence levels** — attribution is a *judgement with stated
   confidence*, not a binary. Overclaiming destroys credibility;
   underclaiming hides the adversary.

The point of attribution is rarely prosecution — it is **defense
prioritisation**: knowing the adversary tells you what they want and
how they will come back.

---

## Malware distribution and communication

The operational spine every campaign needs:

| Element | Question |
|---------|----------|
| Distribution | How does the implant arrive — phishing, supply chain, exposed service? |
| C2 communication | How does it call home, and through what channel? |
| Persistence | How does it survive reboot and reinstall? |

The C2 channel is the defender's best target: the implant must talk,
and the traffic must cross the egress — which is exactly what
[NETWORK_MONITORING](NETWORK_MONITORING.md) watches.

---

## Open-source threat hunting

The equaliser: state TTPs are documented in the open. Public
intelligence feeds, malware repositories, and community writeups let
a defender search their own environment for *known* state behaviour —
no vendor required. The workflow: map the adversary's TTPs → write
the detections → hunt the environment → automate what matched.

---

## Rules of thumb

- **Defend against TTPs, not tools.** The hash changes; the technique
  persists. Detections written against behaviour outlive detections
  written against samples.
- **Attribute with stated confidence.** "Likely APT-X, moderate
  confidence" is a professional judgement; "definitely X" is usually
  wrong and always corrosive.
- **Know what they want.** The adversary's objective determines their
  next move — defend the objective, not the last attack.
- **Hunt, do not wait.** State campaigns sit silent for months; the
  alert that never fires is the campaign that succeeded.

## Why it matters

The mesh's own threat model, read at maximum: a patient, funded
adversary who wants what the mesh holds. The cyberwarfare lens is
what keeps "we are not a target" from becoming the last assumption
made before a breach.
