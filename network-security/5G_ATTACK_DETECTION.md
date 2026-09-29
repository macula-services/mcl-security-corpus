---
title: "Network Security: 5G Attack Detection"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: 5G Attack Detection

*ML-driven detection on 5G and IoT traffic: classify the attack from the traffic's features, because signatures arrive after the damage.*

---

## Why 5G changes the problem

5G and IoT multiply the connected endpoints by orders of magnitude —
and with them, the attack surface: massive device fleets, dense edge
computing, and traffic volumes that make human-in-the-loop analysis
impossible. The classical signature approach breaks down where:

- The protocols are newer than the signatures.
- The volume drowns the analyst.
- The devices cannot run endpoint agents.

The answer the field converged on: **detection as classification over
traffic features**, learned from data.

---

## The ML approach

1. **Dataset** — labelled traffic captures, attack and benign. The
   reference dataset for IoT/5G research is **CICIoT2023**: 33 attack
   types across several categories (DoS, DDoS, recon, web-based,
   spoofing, brute force), captured with realistic IoT traffic.
2. **Feature extraction** — per-flow statistics (packet counts,
   lengths, inter-arrival times, protocol ratios), not payloads.
3. **Classification** — train models (ensemble, deep learning) to
   map feature vectors to attack classes — including **multi-class**
   labels that name the specific attack, not just "anomalous".
4. **Deployment** — the classifier scores live flows at the network
   edge.

The payload-stripped, feature-based design matters: it works on
encrypted traffic, and it generalises across devices.

---

## What the research covers

| Direction | Question |
|-----------|----------|
| **Multi-model classification** | Ensemble/deep models distinguishing 30+ attack classes |
| **Attack path discovery** | Bidirectional analysis tracing how an attack moves through the network |
| **Malware over 5G** | Classifiers over byte features, PE structure, and execution behaviour |
| **User plane deployment** | Where to place the UPF/detection at the edge — detection as a placement problem |
| **Covert channels** | Federated learning + blockchain-based hiding of data — and its detection |

---

## Rules of thumb

- **Feature-based, not payload-based.** Encrypted 5G traffic has no
  payload to inspect; flow statistics are the only durable signal.
- **The dataset is the foundation.** Detection quality is a property
  of the training data — CICIoT2023's value is its realism, and any
  deployment must re-baseline on its own traffic.
- **Name the attack, not just the anomaly.** "DoS" tells the
  operator what to do; "traffic looks odd" does not.
- **Detection at the edge.** Classify close to the device; shipping
  every flow to a centre defeats the point at 5G scale.

## Why it matters

The mesh's own edge — stations, sensors, whatever devices join the
fleet — is exactly the 5G/IoT-shaped surface this note covers. The
pattern transfers: when signatures cannot keep up and humans cannot
keep watch, the answer is a classifier over flow features, trained on
labelled traffic, running at the edge.
