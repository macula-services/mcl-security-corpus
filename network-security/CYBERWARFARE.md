---
title: "Network Security: Cyberwarfare"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: Cyberwarfare

*State-backed operators are the top of the threat model: well funded, patient and willing to wait. Defending against them is about behaviour, visibility and honest attribution.*

---

## What it is

"Cyberwarfare" here covers intrusion campaigns run by, or on behalf of,
governments, and the criminal groups that states tolerate or use. What
separates them from opportunistic attackers is budget and time. They can
stay quiet in a network for months, develop or buy unknown
vulnerabilities, and discard infrastructure once it is spotted. The usual
defender assumption that attackers give up when things get expensive does
not hold.

| Motive | Typical goal | What defenders see |
|--------|--------------|--------------------|
| Espionage | Long-term access to data or communications | Low-volume, persistent access; careful credential use |
| Sabotage and pre-positioning | Ability to disrupt infrastructure later | Footholds in operational or management networks with little activity |
| Revenue generation | Theft, fraud, ransomware for a state or a tolerated group | Hands-on-keyboard intrusion, data staging, extortion |
| Influence | Undermining trust in institutions or elections | Leaks, defacement, account takeover of public figures |

---

## How campaigns are structured

Every campaign, however advanced, needs the same basic elements, and each
is a detection opportunity.

| Element | Defensive question | Where to look |
|---------|--------------------|---------------|
| Initial access | How could an outsider first get code or credentials in? | Mail filtering, exposed-service inventory, supplier access, patch state |
| Command and control | How would an implant receive instructions? | Egress flow records, DNS, TLS metadata, beacon-like regularity |
| Persistence | How would it survive reboots and password resets? | Autorun locations, scheduled tasks, new accounts, altered services |
| Lateral movement | How would it reach the valuable systems? | East-west traffic, unusual admin-protocol use, authentication logs |
| Collection and exit | How would data leave? | Large or unusual outbound transfers, archive creation, new cloud destinations |

Command and control is often the most reliable place to catch an
operation: the implant has to communicate, and that traffic crosses a
boundary that [NETWORK_MONITORING](NETWORK_MONITORING.md) can watch.
MITRE ATT&CK catalogues the observed techniques for each element.

---

## Attribution

Attributing a campaign to an actor is an analytic judgement, not a
forensic fact. Analysts weigh:

- **Behaviour**: the techniques, tooling and working habits observed.
  These change slower than file hashes or IP addresses.
- **Overlap**: shared infrastructure, code reuse, or matching behaviour
  across separate incidents.
- **Context**: who benefits, what was targeted, timing of activity.

Each piece can be faked or coincidental, so conclusions carry an explicit
confidence level ("moderate confidence this is group X"). For most
organisations the value of attribution is not naming a culprit but
predicting what the actor wants and how it is likely to return, so that
defences go where they matter.

---

## Threat hunting with open intelligence

Much about state-linked groups is public: government advisories, ATT&CK
group pages, vendor and community reports. A small team can use it
without paid feeds:

1. Pick the actors relevant to your sector and data.
2. List the behaviours they are documented to use.
3. Check whether your telemetry can see each behaviour at all.
4. Write detections for the gaps, then search historical data.
5. Keep the detections that work; record the ones you cannot build.

---

## Trade-offs and pitfalls

- **Behaviour over indicators.** Blocklists of hashes and addresses age in
  days; detections built on technique last longer but cost more to tune.
- **Overclaiming attribution** damages credibility and can misdirect
  defence; underclaiming can hide a real pattern.
- **"We are too small to be a target"** ignores supply-chain and
  stepping-stone use of smaller organisations.
- **Silence is not safety.** Espionage campaigns are designed not to trip
  alerts; hunting finds what alerting misses.

---

## Relation to the rest of the corpus

This note sets the upper bound for the mesh threat model: a funded,
patient adversary interested in what the mesh carries.
[NETWORK_MONITORING](NETWORK_MONITORING.md) supplies the evidence base for
hunting, and [5G_ATTACK_DETECTION](5G_ATTACK_DETECTION.md) covers automated
detection at the edge.

## Sources

- *The Art of Cyberwarfare: An Investigator's Guide to Espionage, Ransomware, and Organized Cybercrime*, Jon DiMaggio, No Starch Press, 2022. <https://nostarch.com/art-cyberwarfare>
- MITRE ATT&CK, Command and Control tactic (TA0011). <https://attack.mitre.org/tactics/TA0011/>
- MITRE ATT&CK, Groups (tracked threat actors and their techniques). <https://attack.mitre.org/groups/>
- CISA, Nation-State Threats. <https://www.cisa.gov/topics/cyber-threats-and-advisories/nation-state-cyber-actors>
