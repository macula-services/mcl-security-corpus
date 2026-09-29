---
title: "Hardware Security: Fault Injection and Side Channels"
layer: guide
audience: [agent, human]
stage: stable
---

# Hardware Security: Fault Injection and Side Channels

*The chip leaks and the chip can be tripped. Fault injection breaks execution mid-step; power analysis reads secrets from the supply current. The two attacks every secure element must survive.*

---

## The two attack families

| Family | What it is | What it yields |
|--------|------------|----------------|
| **Fault injection** | Disturb the device — voltage, clock, EM pulse — to make execution *skip or corrupt* a step | Skipped signature checks, dumped secrets, a comparison that always passes |
| **Side-channel analysis** | Observe physics — power draw, timing, EM emissions — to *infer* what executes | Keys recovered from power traces, byte by byte |

The hardware attack model in one line: **a device that runs correct
code correctly still leaks** (side channels), and **a device under
physical stress does not run correct code** (faults).

---

## Fault injection — the glitch

The classic target: a branch that must be taken, skipped — the
"is signature valid?" check, the "is firmware authenticated?" gate.
Methods:

| Method | What is disturbed |
|--------|-------------------|
| **Voltage glitching** | Drop the supply for nanoseconds — the CPU skips or corrupts an instruction |
| **Clock glitching** | A too-fast edge — setup violations, skipped steps |
| **EM fault injection** | A pulse near the chip — same effect, no electrical contact |

The workflow: identify the moment (trigger on the target operation),
vary the glitch parameters (width, depth, timing), and observe —
a glitch that flips the outcome is the finding. The Trezor wallet
memory dump is the book's worked example of the payoff.

---

## Power analysis — reading the current

The chip's power draw depends on what it computes: the current trace
of an AES round differs per key byte.

| Analysis | Method | Effort |
|----------|--------|--------|
| **SPA (simple)** | Read the trace directly — visible patterns (key schedules, branches) | Low — one trace |
| **DPA (differential)** | Many traces + statistics: correlate trace segments with a key-byte hypothesis | High — thousands of traces, but recovers keys from noise |

DPA is the one that breaks real products: the attacker hypothesises a
key byte, predicts the power it would cause, and correlation across
traces confirms or rejects the hypothesis — byte by byte.

---

## The defenses

| Attack | Countermeasure |
|--------|----------------|
| Voltage/clock glitching | Glitch detectors, redundant critical branches, hardened clocking |
| EM fault injection | Shielding, layout, detectors |
| SPA | Constant-time and constant-power code, no data-dependent branches |
| DPA | Masking (split secrets so traces are uncorrelated), noise injection, key rotation |

The principle: **the countermeasure must live in the silicon and the
code** — a secure element is a co-design, not a feature added after.

## Rules of thumb

- **Physical access is full access, unless designed otherwise.** The
  device must assume the attacker has probes, a power supply, and time.
- **The glitchable branch is a vulnerability.** Every security check
  must assume the comparison may be skipped — defend the *result*,
  not the check.
- **Assume the trace leaks.** Code that handles secrets must be
  constant-time and masked; optimisation is the enemy (compiler
  "improvements" have broken masked implementations).

## Why it matters

Any device the mesh trusts — a secure element, a hardware key store,
a tamper-evident node — is being measured against exactly these two
attacks by whoever wants in. The note is the check: before trusting
hardware, ask what its fault response and its leakage profile are.
