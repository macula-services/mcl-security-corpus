---
title: mcl-security-corpus — Glossary
layer: glossary
audience: [agent, human]
stage: stable
---

# Glossary

Canonical vocabulary for secure design, web and network security,
pentesting, reversing, and hardware security. One term, one meaning.

---

## Secure design

| Term | Meaning |
|------|---------|
| **Asset** | Valuable data or resources that need protection. |
| **Attack surface** | Every place an attack could originate: inputs, interfaces, endpoints, ports. |
| **Trust boundary** | An interface between more-trusted and less-trusted parts — where validation happens. |
| **Threat** | A way an attacker could harm an asset. |
| **STRIDE** | The threat categories: Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege. |
| **Threat modeling** | The systematic process: model, enumerate, rank, mitigate, verify. |
| **Least privilege** | Grant exactly the rights a task needs, nothing more. |
| **Defense in depth** | Multiple independent layers of defense, so one failure is not total. |

## Web security

| Term | Meaning |
|------|---------|
| **Injection** | Untrusted input interpreted as code (SQL, command, template). |
| **Parameterized query** | Query structure fixed at prepare time, data supplied separately — the injection fix. |
| **XSS** | Foreign script executing in a victim's page: stored, reflected, or DOM-based. |
| **CSRF** | The attacker's page riding the victim's authenticated session to make state-changing requests. |
| **Anti-CSRF token** | A per-session secret required on state-changing requests, unreadable cross-origin. |
| **SameSite** | Cookie attribute refusing to send the cookie on cross-site requests. |
| **Session fixation** | The session id stays the same across privilege change, letting the attacker inherit it. |
| **HttpOnly** | Cookie flag making it unreadable to script. |

## Network security

| Term | Meaning |
|------|---------|
| **NSM** | Network security monitoring: collecting and analysing network data for intrusion detection and response. |
| **Full content data** | Raw packets — complete detail, massive volume. |
| **Session data** | Conversation summaries: who, when, how much. The workhorse. |
| **Transaction data** | Application-level records: DNS, HTTP, email. |
| **Statistical data** | Aggregates over time — the baseline for anomaly. |
| **Alert data** | What detection tools flagged — hypotheses to verify. |
| **Sensor** | A collection point watching one network segment. |
| **Egress** | Outbound traffic — the chokepoint where exfiltration shows up. |

## Pentest

| Term | Meaning |
|------|---------|
| **Pentest** | Simulated attack assessing what an attacker would gain. |
| **Vulnerability assessment** | Finding vulnerabilities without exploiting them. |
| **Scope** | The agreed targets and allowed actions. |
| **Rules of engagement** | What the tester may and may not do. |
| **Reconnaissance** | Information gathering about the target. |
| **Exploitation** | Using a vulnerability to gain access. |
| **Post-exploitation** | Leveraging the foothold: further data, further systems. |
| **BOLA / IDOR** | Broken object level authorization: changing an object id to read another's data. |
| **Mass assignment** | Binding request fields to object fields the client was never meant to control. |
| **Fuzzing** | Feeding unexpected input and watching the response. |
| **Zero-day** | A vulnerability unknown to (and unpatched by) the vendor. |

## Reversing

| Term | Meaning |
|------|---------|
| **Disassembly** | Machine code → assembly instructions. |
| **Decompilation** | Assembly → high-level pseudocode. |
| **Strings pass** | Extracting embedded printable strings — the cheapest first step. |
| **Cross-reference (xref)** | Who calls this function, what touches this data. |
| **Annotation** | Naming and typing what the tool recovered — the actual work of reversing. |
| **ELF / PE / Mach-O** | Executable formats: Linux, Windows, macOS. |

## Hardware security

| Term | Meaning |
|------|---------|
| **UART** | Serial console — a root shell if left enabled. |
| **JTAG** | Chip-level debug: halt the CPU, read memory, dump firmware. |
| **SWD** | ARM's two-wire JTAG equivalent. |
| **SPI / I2C** | Serial buses between chips — where secrets cross unencrypted. |
| **Firmware** | The device's embedded software — the dump target. |
| **Logic analyser** | Captures bus traffic for analysis. |
| **RFID** | Short-range radio tags — clone, relay, spoof. |
