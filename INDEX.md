---
title: mcl-security-corpus — Index
layer: index
audience: [agent, human]
stage: stable
---

# mcl-security-corpus

*Knowledge corpus for security: secure design, web and network defense, pentesting, reversing, and hardware security.*

This index maps every note in the corpus. An agent answering a question
recalls from this repo through mcl-rag; this file is the human-readable
map of what is here and where a fuller answer lives.

---

## Domain map

### Secure design — `secure-design/`

Designing software that resists attack.

| Note | Covers |
|------|--------|
| [README](secure-design/README.md) | The design discipline in one page |
| [SECURE_DESIGN](secure-design/SECURE_DESIGN.md) | STRIDE, the four questions, the threat modeling process |

### Web security — `web-security/`

The web's classic vulnerabilities and their defenses.

| Note | Covers |
|------|--------|
| [README](web-security/README.md) | The stable classes in one page |
| [WEB_SECURITY](web-security/WEB_SECURITY.md) | Injection, XSS (stored/reflected/DOM), CSRF, sessions |

### Network security — `network-security/`

Monitoring the network to find intruders.

| Note | Covers |
|------|--------|
| [README](network-security/README.md) | NSM in one page |
| [NETWORK_MONITORING](network-security/NETWORK_MONITORING.md) | The five data types, deployment, the detection workflow |

### Pentest — `pentest/`

Authorized offensive testing.

| Note | Covers |
|------|--------|
| [README](pentest/README.md) | The discipline in one page |
| [PENTEST_METHODOLOGY](pentest/PENTEST_METHODOLOGY.md) | The seven stages, and the authorization that makes it legal |
| [API_TESTING](pentest/API_TESTING.md) | Discovery, authn/authz, fuzzing, mass assignment, rate limits, GraphQL |

### Reversing — `reversing/`

Understanding binaries from their bytes.

| Note | Covers |
|------|--------|
| [README](reversing/README.md) | The discipline in one page |
| [REVERSE_ENGINEERING](reversing/REVERSE_ENGINEERING.md) | The workflow: format, load, disassemble, decompile, annotate |

### Hardware security — `hardware-security/`

The attack surface below the OS.

| Note | Covers |
|------|--------|
| [README](hardware-security/README.md) | The device surface in one page |
| [HARDWARE_HACKING](hardware-security/HARDWARE_HACKING.md) | UART/JTAG/SWD, SPI/I2C, firmware, radio — and the defenses |

---

## Reading path — agent

1. [`GLOSSARY.md`](GLOSSARY.md) for the vocabulary.
2. The domain README closest to the question.
3. The specific note, if the README points at one.

## Reading path — human

1. [`README.md`](README.md) → this index → the domain that interests you.
2. Notes are short and standalone; there is no required order.
