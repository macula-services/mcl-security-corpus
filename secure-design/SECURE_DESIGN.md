---
title: "Secure Design: Threat Modeling"
layer: guide
audience: [agent, human]
stage: stable
---

# Secure Design: Threat Modeling

*Threat modeling is structured pessimism applied early: describe the system, list how it could be abused, decide what to do about each abuse, and check that the decision held.*

---

## What it is

A threat model is a written answer to "how could this system be made to
do something its owners did not intend?", produced while the design can
still change. It is cheap on a whiteboard and expensive in production:
a missing trust boundary found in review costs a diagram edit, the same
gap found after release costs a redesign.

The community consensus, captured in the Threat Modeling Manifesto,
reduces the practice to four questions:

1. What are we working on?
2. What can go wrong?
3. What are we going to do about it?
4. Did we do a good enough job?

Every method (STRIDE, attack trees, kill chains, LINDDUN for privacy) is
a way of answering question 2 more systematically. Questions 3 and 4 are
where most teams stop short.

---

## Building blocks

| Concept | Working definition | Why it matters to design |
|---------|--------------------|--------------------------|
| **Asset** | Something worth protecting: data, a capability, a reputation, uptime | Without named assets there is nothing to rank against |
| **Attack surface** | Every point where input from outside a component arrives: APIs, files, queues, config, UI | Removing an entry point removes the threats that use it |
| **Trust boundary** | A line where the level of trust changes, such as client to server, tenant to tenant, service to database | Validation, authentication and authorization belong on this line |
| **Threat** | A plausible way an adversary harms an asset through the attack surface | The unit you rank and mitigate |
| **Mitigation** | A control that removes, reduces or detects a threat | Must be testable, otherwise it is an assumption |

A useful habit: draw the data-flow diagram first, then draw the trust
boundaries as dashed lines across it. Every arrow crossing a dashed line
is a question.

---

## STRIDE as a checklist

STRIDE, which originated at Microsoft, gives six prompts to ask of each
element and each boundary-crossing flow. Each maps to a security
property it violates:

| Threat | Property violated | Example prompt for a message-queue consumer |
|--------|------------------|---------------------------------------------|
| **S**poofing | Authenticity | Can a producer claim to be a different service? |
| **T**ampering | Integrity | Can a message be altered between broker and consumer? |
| **R**epudiation | Non-repudiation | If a bad message is processed, can we prove who sent it? |
| **I**nformation disclosure | Confidentiality | Do messages or dead-letter queues leak personal data? |
| **D**enial of service | Availability | Can one producer flood the queue and starve others? |
| **E**levation of privilege | Authorization | Can a crafted message make the consumer act with its own higher rights? |

The value of STRIDE is coverage, not insight: it stops the exercise
from depending on who happens to be in the room.

---

## Running it in practice

- **Scope** the model to something a team can hold in their heads: one
  service, one feature, one integration.
- **Enumerate** threats per element with STRIDE, then **rank** them by
  impact and likelihood (a simple high/medium/low grid is enough; the
  point is ordering, not precision).
- **Decide** per threat: mitigate, eliminate (remove the feature or
  input), transfer, or accept with a named owner.
- **Verify**: every mitigation gets a test, a review item or a
  monitoring signal. Question 4 is answered by evidence, not by
  sign-off.
- **Revisit** when the design changes. A threat model is a living
  artefact tied to the architecture, not a one-time document.

Testers use the model as a map: the ranked threat list becomes the test
plan for a [pentest](../pentest/PENTEST_METHODOLOGY.md), and unmitigated
or accepted threats become detection requirements for
[monitoring](../network-security/NETWORK_MONITORING.md).

---

## Pitfalls

- **Modeling after the code exists.** The model then documents
  boundaries instead of shaping them.
- **Chasing completeness.** Any real system has more threats than time.
  Ranking is the discipline; an exhaustive list nobody acts on is waste.
- **Mitigations that cannot be checked.** "Input is validated" means
  nothing without saying where, against what, and how it is tested.
- **Ignoring elimination.** The strongest mitigation is often to not
  accept the input, open the port or store the data at all.
- **Validating in the wrong place.** Checks belong where data crosses
  into the more-trusted side, not scattered at arbitrary layers.

## Related notes

[DEFENSE_TACTICS](DEFENSE_TACTICS.md) covers the operational posture that
complements design-time modeling; [WEB_SECURITY](../web-security/WEB_SECURITY.md)
lists the classic flaws a web threat model should always cover.

## Sources

- *Designing Secure Software: A Guide for Developers*, Loren Kohnfelder, No Starch Press, 2021. https://nostarch.com/designing-secure-software
- Threat Modeling Manifesto (source of the four questions). https://www.threatmodelingmanifesto.org/
- OWASP Threat Modeling Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html
- Microsoft Threat Modeling Tool: Threats (STRIDE categories). https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats
