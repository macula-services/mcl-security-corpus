---
title: "Network Security: Monitoring (NSM)"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: Monitoring (NSM)

*Assume some attacks will get past prevention; keep enough network evidence to notice them, scope them and prove what happened.*

---

## What it is

Network security monitoring (NSM) is the practice of collecting network
traffic records, analysing them for signs of intrusion, and using them to
drive incident response. It starts from a design assumption rather than a
product: filters and signatures block what they already recognise, so an
organisation also needs a way to find what they missed. NSM supplies that
second line with evidence instead of guesswork.

It is distinct from intrusion prevention. An IPS decides in real time and
drops traffic; NSM keeps records so that a human or a later query can
reconstruct events hours or weeks afterwards.

---

## Kinds of network evidence

Each kind of record trades detail against storage cost and retention time.

| Record | Contents | Typical use | Cost |
|--------|----------|-------------|------|
| **Packet capture** | Every byte on the wire | Final proof, payload analysis (when not encrypted) | Very high; retained for days at most |
| **Flow / session records** | Endpoints, ports, start and end time, byte and packet counts | First pivot in almost every investigation | Low; retain for months |
| **Protocol logs** | Parsed application events: DNS lookups, HTTP requests, TLS handshake metadata, file transfers | Meaning without payload bulk | Moderate |
| **Aggregates and statistics** | Counts and volumes per host, port or time window | Baselines, trends, anomaly spotting | Very low |
| **Detector alerts** | Signature or heuristic hits from an IDS | Starting points for triage | Low, but noisy |

An example: an IDS alerts that a workstation fetched a file matching a
known malicious pattern. Flow records show the same host has since opened
a small, regular outbound connection every 60 seconds to one address.
Protocol logs show the address was reached through a newly registered
domain. If packet capture is still retained, it confirms the content. Each
layer narrows the question for the next.

---

## Where sensors go

A sensor sees only traffic that passes its tap or span port, so placement
decides coverage.

- **Egress points** to the internet: catch outbound command-and-control and
  data leaving.
- **In front of high-value segments**: databases, identity systems, build
  infrastructure.
- **Between trust zones**: east-west traffic is where lateral movement shows.

Small environments start with one sensor. Larger ones run a sensor per
segment feeding a central store and console. Encrypted traffic limits
payload inspection, which pushes the weight onto flow records, DNS and TLS
metadata, and endpoint telemetry.

---

## The investigation loop

1. **Triage the trigger**: an alert, a user report, a threat-intel match.
2. **Widen with flow records**: what else did this host talk to, and when?
3. **Add protocol context**: names resolved, URLs requested, certificates seen.
4. **Confirm with content** if packet capture exists for that window.
5. **Correlate beyond the network**: host logs, proxy logs, authentication
   events, EDR. The network is one vantage point.
6. **Feed back**: turn what was learned into a new detection or a tuned one.

---

## Trade-offs and pitfalls

- **Retention is a security decision.** Evidence not kept cannot be
  searched later; decide retention per record type, not by disk defaults.
- **Alerts are leads, not verdicts.** Untuned signatures bury analysts;
  measure false-positive rates and prune.
- **No baseline, no anomaly.** Statistical detection only works against a
  recorded picture of normal traffic.
- **Blind spots are silent.** Document which segments have no sensor and
  which traffic is encrypted end to end.
- **Privacy and law.** Full capture can contain personal data; limit access
  and retention accordingly.

---

## Relation to the rest of the corpus

[CYBERWARFARE](CYBERWARFARE.md) describes patient adversaries whose
command channels NSM is well placed to spot.
[5G_ATTACK_DETECTION](5G_ATTACK_DETECTION.md) applies learned
classification to the same flow features. For the mesh, every station
link is traffic that is either watched or not; NSM is how "we have a
firewall" becomes "we would notice and could prove an intrusion".

## Sources

- *The Practice of Network Security Monitoring: Understanding Incident Detection and Response*, Richard Bejtlich, No Starch Press, 2013. <https://nostarch.com/nsm>
- NIST SP 800-94, *Guide to Intrusion Detection and Prevention Systems (IDPS)*, Karen Scarfone and Peter Mell, NIST, 2007. <https://csrc.nist.gov/pubs/sp/800/94/final>
- NIST SP 800-61 Rev. 3, *Incident Response Recommendations and Considerations for Cybersecurity Risk Management: A CSF 2.0 Community Profile*, NIST, 2025. <https://csrc.nist.gov/pubs/sp/800/61/r3/final>
- Zeek documentation, log reference (conn, dns, http and other protocol logs). <https://docs.zeek.org/en/master/logs/index.html>
