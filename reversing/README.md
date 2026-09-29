---
title: Reversing
layer: index
audience: [agent, human]
stage: stable
---

# Reversing

*Disassembly and decompilation: understanding binaries from their bytes.*

---

## Notes

| Note | Covers |
|------|--------|
| [REVERSE_ENGINEERING](REVERSE_ENGINEERING.md) | The workflow: format, load, disassemble, decompile, annotate |

## One-page summary

Reverse engineering reads meaning out of a binary: identify the format,
load it, let the disassembler turn machine code into instructions and
the decompiler turn instructions into C-like pseudocode, then annotate
— naming functions, variables, and data structures until the program's
logic reads like the source that was never shipped. The tools are
Ghidra and IDA; the skill is the annotation.
