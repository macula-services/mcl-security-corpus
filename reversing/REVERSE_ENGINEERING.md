---
title: "Reversing: The Workflow"
layer: guide
audience: [agent, human]
stage: stable
---

# Reversing: The Workflow

*Disassembly turns machine code into instructions; decompilation turns instructions into pseudocode; annotation turns pseudocode into understanding. The tool does the first two — you do the third.*

---

## The stages

### 1. Identify the format

Before analysis: what *is* the file? Executable format (ELF, PE,
Mach-O), architecture (x86-64, ARM, …), word size, endianness. The
format tells the tool how to load it; loading it wrong analyses the
wrong bytes.

### 2. Load and analyse

The tool builds the program's structure: entry point, sections, import
and export tables, strings, cross-references. The **strings** pass is
usually the first payoff — embedded URLs, format strings, error
messages, commands — before any instruction is read.

### 3. Disassemble

Machine code → assembly. Straight-line code is easy; the hard part is
**what is code vs data**, and where functions begin. Modern
disassemblers solve most of this with recursive descent and heuristics
— but their mistakes are where analysis time goes.

### 4. Decompile

Assembly → C-like pseudocode. The decompiler is the great equalizer
for new reversers: control flow and data access become readable at a
glance. Treat it as *a guess the tool made* — cross-check against the
disassembly whenever the logic matters.

### 5. Annotate — the actual skill

The tool does not know what anything *means*. The work of reversing
is:

- **Name** functions (`verify_license`, `decrypt_payload`) and
  variables.
- **Type** data structures — recover struct layouts from how offsets
  are accessed.
- **Trace** the cross-references: who calls this, what writes that
  buffer.
- **Record** the findings in comments; the annotated database is the
  deliverable.

A fully annotated binary reads like source. The annotation is the
difference between a disassembly and a reverse engineering.

---

## The questions reversing answers

| Context | Question |
|---------|----------|
| Malware analysis | What does it do, and how do we detect it? |
| Vulnerability research | How does this parser fail on bad input? |
| Interoperability | What protocol does this closed device speak? |
| License/DRM | What does the check actually check? |
| Legacy software | What did this binary really do? (the source is gone) |

---

## Rules of thumb

- **Strings first, always.** The cheapest intelligence in the file.
- **Trust the decompiler, verify at the disassembly.** They disagree
  exactly where the interesting bugs are.
- **Name as you go.** A name given early compounds; an unnamed
  function read twice is time lost twice.
- **Cross-references are the map.** Reading a function in isolation
  explains nothing; the callers and the data flows explain everything.
- **Stay version-agnostic.** The tools evolve; the workflow (format →
  load → disassemble → decompile → annotate) does not.

## Why it matters

Everything closed on the mesh — a proprietary driver, a blob in a
device, a suspicious binary on a station — yields to the same
workflow. Reversing is how "we do not know what this does" becomes
"here is what it does, annotated" — and the annotation is what makes
the knowledge shareable.
