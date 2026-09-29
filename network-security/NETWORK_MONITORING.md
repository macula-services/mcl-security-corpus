---
title: "Network Security: Monitoring (NSM)"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: Monitoring (NSM)

*Collect the network's own data, analyse it, respond. The five data types are the trade: how much detail can you afford against how much volume you carry.*

---

## Why monitor

Prevention fails — that is not cynicism, it is the design premise.
Firewalls and signatures stop what they know; NSM exists for what they
do not. Network security monitoring is **the collection and analysis
of network data to detect and respond to intrusions** — the layer that
answers "are we owned?" from evidence, not from hope.

---

## The five data types

| Type | What it is | Trade |
|------|------------|-------|
| **Full content** | The raw packets — every byte | Total detail, massive volume; kept only briefly |
| **Session data** | Summaries of conversations: who talked to whom, when, how much | The first thing to query; cheap to keep long |
| **Transaction data** | Application-level records (DNS queries, HTTP requests, emails) | Protocol meaning without payload bulk |
| **Statistical data** | Aggregates over time: counts, bytes, connections per host | Trend and anomaly; the baseline |
| **Alert data** | What the detection tools flagged (IDS signatures, heuristics) | Judgement pre-applied — verify, never trust blindly |

The workflow follows the ladder: an **alert** names a moment, the
**session data** shows the conversation, **transaction data** shows
what was said, **full content** shows everything — if you kept it.

---

## Deployment shapes

| Shape | When |
|-------|------|
| **Stand-alone** | One sensor watching one segment — the starting point |
| **Distributed** | Sensors at each segment, one central console — the scaling shape |

Placement is the whole game: a sensor sees only the traffic that passes
it. Watch the chokepoints — internet egress, the segment in front of
the crown jewels — because that is where the traffic *is*.

---

## The detection workflow

1. **Alert fires** — something matched a signature or heuristic.
2. **Pivot to session data** — what else did that host do, with whom,
   when?
3. **Pull transaction records** — which requests, which DNS lookups?
4. **Reach for full content** — the actual bytes, if retention allows.
5. **Cross-correlate logs** — server logs, proxy logs, endpoint data —
   the network view is one view.

---

## Rules of thumb

- **Collect before you need it.** Retrospective analysis is only
  possible if the data was captured; retention policy is a security
  decision, not a storage decision.
- **Session data is the workhorse.** Cheap, long-lived, and enough to
  answer most questions; keep it longer than everything else.
- **Alerts are hypotheses.** A signature match is the *start* of an
  investigation, never its conclusion.
- **Baseline the normal.** Statistical data means nothing without a
  normal to compare against; anomaly detection is baseline detection.

## Why it matters

The mesh's stations are boxes on networks: every node, every service,
every station link is traffic that either is or is not being watched.
NSM is the discipline that turns "we have a firewall" into "we would
see an intruder within hours, and prove what they did".
