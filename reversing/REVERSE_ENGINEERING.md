---
title: "Reversing: The Workflow"
layer: guide
audience: [agent, human]
stage: stable
---

# Reversing: The Workflow

*Reverse engineering recovers what a binary does when the source is missing or untrusted. Tools recover structure; the analyst recovers meaning.*

---

## What it is

Reverse engineering (RE) is the analysis of compiled code to learn its
behaviour without its source. Defenders use it to understand malware,
audit third-party components, check that a shipped build matches what
was reviewed, and keep legacy systems running after the source is lost.
It is analysis of a binary you hold and are allowed to examine; it is
not, in this corpus, a route to breaking someone else's system.

## How it works

Every modern RE tool (Ghidra, IDA, Binary Ninja, radare2) walks the same
pipeline. The names differ; the stages do not.

| Stage | What happens | What can go wrong |
|-------|--------------|-------------------|
| **Triage** | Identify container format (ELF, PE, Mach-O, raw firmware), CPU, word size, byte order; hash the file | A wrong architecture guess produces confident nonsense |
| **Loading** | Map sections to addresses, parse imports, exports, relocations, symbols | Raw firmware has no header: the base address must be inferred |
| **Auto-analysis** | Find functions, follow control flow, collect strings and cross-references | Code and data get confused; indirect jumps hide functions |
| **Disassembly** | Machine code shown as instructions | Correct but low level, slow to read |
| **Decompilation** | Instructions lifted to C-like pseudocode | A reconstruction, not the original: types and loops can be wrong |
| **Annotation** | Analyst names, types and comments the program | This is the work; skipping it leaves an unread listing |

A small illustration: a function that loads a pointer, reads offsets
0x0, 0x8 and 0x10, and passes the value at 0x8 to a length check is
probably handling a struct of `{ptr, len, flags}`. Declaring that struct
once makes every function that touches it readable at the same time.
That compounding effect is why annotation, not disassembly, is the skill.

## How defenders and testers apply it

| Use | Question answered | Output |
|-----|-------------------|--------|
| Malware triage | What does it contact, persist, encrypt? | Indicators and detection rules |
| Component audit | Does this library parse untrusted input safely? | Findings for the vendor, patch requests |
| Build verification | Does the shipped binary match reviewed source? | Diff report, supply-chain assurance |
| Interoperability | What format or protocol does this device use? | A written specification |
| Legacy recovery | What did the lost program actually do? | Documentation for a rewrite |

The deliverable is the annotated project plus a written summary. Share
both; the project lets a colleague verify each claim.

## Trade-offs and pitfalls

- **Strings and imports are cheap first evidence** (URLs, error
  messages, API names) but are easy to fake or encrypt. Treat them as
  leads, not conclusions.
- **Decompiler output is a hypothesis.** Where logic matters, confirm
  it against the instructions.
- **Obfuscation and packing** defeat static analysis until the real code
  is recovered; a binary with very few functions and high entropy is
  usually packed.
- **Dynamic analysis** (running under a debugger or emulator) answers
  questions static reading cannot, but running untrusted code needs an
  isolated, disposable environment.
- **Legal scope.** Licence terms and local law on reverse engineering
  vary; confirm authorisation before starting.

## Related notes

[IDA_PRO](IDA_PRO.md) shows how one tool organises this pipeline.
Firmware pulled from devices in
[HARDWARE_HACKING](../hardware-security/HARDWARE_HACKING.md) is analysed
with exactly this workflow.

## Sources

- *The Ghidra Book: The Definitive Guide*, Chris Eagle and Kara Nance, No Starch Press, 1st ed. 2020 (2nd ed. 2026). [Publisher page (2nd ed.)](https://nostarch.com/ghidra-book-2e)
- *The IDA Pro Book*, 2nd ed., Chris Eagle, No Starch Press, 2011. [Publisher page](https://nostarch.com/idapro2.htm)
- Ghidra, National Security Agency, open source repository and documentation. [GitHub](https://github.com/NationalSecurityAgency/ghidra)
- Ghidra API reference. [ghidra.re](https://ghidra.re/ghidra_docs/api/)
