---
title: "Reversing: IDA Pro"
layer: guide
audience: [agent, human]
stage: stable
---

# Reversing: IDA Pro

*A commercial disassembler and decompiler whose central idea is the analysis database: the binary is input, the database is the work product.*

---

## What it is

IDA Pro, from Hex-Rays, is a long-established interactive disassembler
with optional decompilers for common architectures. It follows the
general [reversing workflow](REVERSE_ENGINEERING.md); what sets it apart
is how it stores and extends analysis. Ghidra is the main open source
alternative and shares most of these ideas.

## How it works

| Idea | Meaning in practice |
|------|---------------------|
| **Analysis database** | Loading a binary creates a database file holding the disassembly plus every name, type and comment. You reopen and share the database, not the binary. |
| **Library recognition (FLIRT)** | Byte-pattern signatures identify statically linked library functions and name them, so analysis time goes to the program's own code. |
| **Type system** | Structs, enums and function prototypes can be declared or imported from headers; decompiler output improves as types are added. |
| **Cross-references** | Every code and data reference is indexed, so "who calls this" and "who writes this buffer" are one lookup. |
| **Views** | Linear listing, control-flow graph, hex, strings, pseudocode: one database, several lenses kept in sync. |
| **Scripting and plugins** | IDAPython (and a C++ SDK) expose the database for automation: bulk renaming, pattern search, custom loaders, reports. |

## How defenders apply it

- **Malware analysis:** let library recognition run, then study what
  remains unnamed, since that is the author's own code. Export findings
  as indicators and detection rules.
- **Patch analysis:** comparing two versions of a vendor binary shows
  which functions a security update changed, which tells defenders what
  was fixed and how urgently to deploy it.
- **Automation:** a script that labels every call to a crypto or network
  API gives a fast map of a large binary; teams keep such scripts under
  version control like any other tooling.
- **Hypothesis testing:** the database can model a changed instruction
  so an analyst can confirm what a check controls in a lab copy of
  software they are authorised to study. The deliverable is the
  explanation, not a modified binary.

## Trade-offs and pitfalls

- **Cost and licensing.** IDA Pro is commercial; the free edition is
  limited. Ghidra is free and scriptable, so tool choice is often a
  budget question more than a capability one.
- **Signatures can mislead.** A wrong library match names a function
  confidently and wrongly; check suspicious matches.
- **Types before refactoring.** Names make cross-references readable,
  types make pseudocode readable; restructuring before either slows
  both.
- **Treat the database as a project artefact.** Back it up and version
  it. It holds hours of reasoning that the binary does not.

## Related notes

[REVERSE_ENGINEERING](REVERSE_ENGINEERING.md) holds the tool-neutral
pipeline. Firmware images from
[HARDWARE_HACKING](../hardware-security/HARDWARE_HACKING.md) often
need a custom loader or a manually set base address before analysis.

## Sources

- *The IDA Pro Book: The Unofficial Guide to the World's Most Popular Disassembler*, 2nd ed., Chris Eagle, No Starch Press, 2011. [Publisher page](https://nostarch.com/idapro2.htm)
- Hex-Rays documentation (IDA user guide). [docs.hex-rays.com](https://docs.hex-rays.com/)
- Hex-Rays, FLIRT signatures. [docs.hex-rays.com](https://docs.hex-rays.com/user-guide/signatures/flirt)
- Hex-Rays, IDAPython SDK. [docs.hex-rays.com](https://docs.hex-rays.com/developer-guide/idapython)
