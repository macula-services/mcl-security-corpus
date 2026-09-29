---
title: mcl-security-corpus
layer: index
audience: [agent, human]
stage: stable
---

# mcl-security-corpus

*Knowledge corpus for security: secure design, web and network defense, penetration testing, reversing, and hardware security. Markdown only — this repo is ingested by [mcl-rag](https://github.com/macula-services/mcl-rag), the mesh's shared memory.*

This repository holds reference knowledge an agent on the Macula mesh can
recall: how to design software that resists attack, how to watch a network
for intruders, how authorized testing is performed, and what the hardware
attack surface looks like. It contains no runtime code and no exploit
payloads — methodology and principles, not recipes.

> **Scope.** This corpus includes offensive methodology (pentest phases,
> reversing workflows) because defense requires knowing how attacks
> proceed. It does not include weaponized payloads, malware, or
> exploitation code. Keep it that way: methodology in, payloads out.
>
> **mcl-rag ingestion notes.** The sync loop fast-forwards this repo and
> re-embeds changed `**/*.md` files every 120 s. A commit is a deploy.

---

## Start here

| You are | Read |
|---------|------|
| Agent, first recall | [`INDEX.md`](INDEX.md) → domain map |
| Human, first contact | [`INDEX.md`](INDEX.md) → read top-down |
| Looking for a term | [`GLOSSARY.md`](GLOSSARY.md) |
| Secure design questions | [`secure-design/`](secure-design/README.md) |
| Web security questions | [`web-security/`](web-security/README.md) |
| Network defense questions | [`network-security/`](network-security/README.md) |
| Pentest / offensive questions | [`pentest/`](pentest/README.md) |
| Reversing questions | [`reversing/`](reversing/README.md) |
| Hardware security questions | [`hardware-security/`](hardware-security/README.md) |

---

## Layout

| Domain | Where | Purpose |
|--------|-------|---------|
| **Secure design** | `secure-design/` | Requirements, threat modeling, design principles |
| **Web security** | `web-security/` | Web vulnerabilities and their defenses |
| **Network security** | `network-security/` | Monitoring, collection, attack detection |
| **Pentest** | `pentest/` | Authorized testing methodology: recon to report |
| **Reversing** | `reversing/` | Disassembly, decompilation, analysis workflows |
| **Hardware security** | `hardware-security/` | Embedded, IoT, and vehicle attack surfaces |

---

## Conventions

- **Front-matter** on every note: `title`, `layer`, `audience`, `stage`.
- **One pattern per note.** Notes are lookup targets for RAG, not books.
- **Methodology in, payloads out.** Describe how attacks proceed and how
  defenses detect them; do not include working exploit code.
- **`stage`** is `draft`, `stable`, or `superseded`. Superseded notes
  link to their replacement instead of being deleted.
- **License:** MIT. Deposit only knowledge you may license as MIT.
