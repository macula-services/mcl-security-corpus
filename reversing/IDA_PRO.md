---
title: "Reversing: IDA Pro"
layer: guide
audience: [agent, human]
stage: stable
---

# Reversing: IDA Pro

*The industry workhorse disassembler: the database is the project, FLIRT names the libraries, scripting extends everything.*

---

## What IDA adds over the generic workflow

The [workflow](REVERSE_ENGINEERING.md) is the same — format, load,
disassemble, decompile, annotate — but IDA shapes how the work is
organised:

- **The database is the project.** All analysis — names, types,
  comments, the disassembly itself — lives in the `.i64` database,
  not in the binary. Sharing the database shares the analysis.
- **FLIRT signatures** recognise statically linked library code, so
  you do not reverse `printf` a thousand times — the library functions
  are named for you, and the *remaining* code is the interesting part.
- **Scripting.** IDAPython drives every aspect: automate the tedious
  parts, batch the annotations, extend the tool.

---

## The workspace concepts

| Concept | What it is |
|---------|------------|
| **Data displays** | The views: disassembly, hex, structure, strings, graph — same data, different lenses |
| **Navigation** | Jumping by address, name, xref — the map is the productivity |
| **Manipulation** | Naming, commenting, converting code to data and back — the annotation loop |
| **Datatypes** | Defining structs and enums; the decompiler's output quality follows the typing work |
| **Xrefs and graphing** | Call graphs and data flows — who reaches this, what touches that |

The daily loop: follow an xref to a function, read it, name it, type
its arguments, follow the next xref. The database compounds.

---

## Extending and patching

| Extension | What it does |
|-----------|--------------|
| **IDAPython** | Full API access — scripts for anything repetitive |
| **Plugins** | Compiled extensions for heavier work (decompilers, diffing) |
| **Patching** | Edit the binary through the database — the "test the hypothesis" move (patch a branch, rerun) |

Patching is not the deliverable — it is the *experiment*: change the
check, run it, observe, and you have proven what the check does.

---

## Rules of thumb

- **Let FLIRT work first.** Any code still unnamed after library
  recognition is either custom or packed — both are the interesting
  cases.
- **Name, then type, then refactor.** Names unlock xrefs; types unlock
  the decompiler; premature structure slows both.
- **Script the third repetition.** IDAPython exists so no human
  renames a hundred functions by hand.
- **The database is the artifact.** Back it up like the source repo —
  the analysis is the work product, the binary was only the input.

## Why it matters

When the mesh needs to know what a closed binary does — a driver, a
firmware blob, a suspicious sample on a station — IDA is the tool
where that analysis accumulates. The note's point: the tool gives you
views and scripts; the *database* is where understanding is stored and
shared.
