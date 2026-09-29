---
title: "Network Security: 5G Attack Detection"
layer: guide
audience: [agent, human]
stage: stable
---

# Network Security: 5G Attack Detection

*At 5G and IoT scale, signatures and human review cannot keep up; learned classifiers over flow statistics can, if the training data is honest and the model is placed near the edge.*

---

## What changes with 5G and IoT

5G networks connect far more devices per area than earlier generations,
push computing out to edge sites, and carry much of their traffic
encrypted. Many connected devices are cheap sensors that cannot run an
endpoint agent. Three consequences for defenders:

- New protocols and device types appear faster than signatures are written.
- Traffic volume exceeds what analysts can review.
- The device itself is often not a place detection can run.

A common response in research and practice is to treat detection as a
**classification problem over traffic features**: learn, from labelled
examples, what benign traffic and each attack type look like.

---

## How it works

1. **Labelled data.** Captures of benign traffic and of known attack
   classes (flooding, scanning, spoofing, brute force, botnet activity).
   A widely used public dataset is CICIoT2023, recorded from a testbed of
   105 IoT devices with 33 attacks grouped into seven classes.
2. **Features.** Per-flow or per-window statistics such as packet counts,
   size distributions, timing between packets, flag and protocol mixes.
   No payload is needed, so the approach still works when traffic is
   encrypted.
3. **Model.** Tree ensembles and neural networks are typical. Output can be
   binary (benign or attack), a coarse class, or a specific attack label.
4. **Deployment.** The model scores live flows, ideally close to where
   traffic enters the network, for example alongside the user plane
   function at an edge site.

Example: a smart meter normally sends a few small packets every few
minutes. A burst of thousands of identical short connections per second
from that meter produces a feature vector far from its baseline, and a
multi-class model can label it as flooding rather than just "unusual",
which tells the operator to rate-limit or isolate the device.

---

## Research directions worth knowing

| Direction | Defensive question |
|-----------|--------------------|
| Fine-grained multi-class detection | Can the model tell dozens of attack types apart reliably? |
| Tracing attack progression | Can detections be linked to show how an attacker moved between devices and segments? |
| Malware classification | Can samples reaching devices be classified from static structure and behaviour? |
| Detector placement | Where in the core and edge should detection run for coverage and latency? |
| Federated and privacy-preserving training | Can sites train together without sharing raw traffic, and how is that training itself protected from poisoning or covert data leakage? |

---

## Trade-offs and pitfalls

- **Dataset bias.** Lab datasets are cleaner than production. Accuracy
  reported on a public dataset rarely survives contact with a real
  network; retrain or at least recalibrate on local traffic.
- **Unknown attacks.** A classifier recognises what it was trained on.
  Pair it with anomaly detection and human review for novel behaviour.
- **Adversarial evasion.** Attackers can shape traffic to look benign;
  test models against deliberate perturbation and monitor drift.
- **Explainability.** Operators act faster on "flood from device X" than
  on a score; prefer models and outputs that give a reason.
- **Scale versus centralisation.** Shipping every flow to a central
  classifier defeats the latency and bandwidth benefits of the edge.

---

## Relation to the rest of the corpus

This is [NETWORK_MONITORING](NETWORK_MONITORING.md) with an automated
analyst: the same flow records, scored by a model instead of read by a
person. Stations and devices at the mesh edge have the same shape of
problem, so the pattern carries over: flow features, honest training
data, classification close to the traffic, and a human loop for what
the model has not seen.

## Sources

- Euclides Carlos Pinto Neto, Sajjad Dadkhah, Raphael Ferreira, Alireza Zohourian, Rongxing Lu, Ali A. Ghorbani, "CICIoT2023: A Real-Time Dataset and Benchmark for Large-Scale Attacks in IoT Environment", *Sensors* 23(13), 5941, MDPI, 2023. Open access under CC BY 4.0. <https://doi.org/10.3390/s23135941>
- Canadian Institute for Cybersecurity, University of New Brunswick, CIC IoT Dataset 2023. <https://www.unb.ca/cic/datasets/iotdataset-2023.html>
- 3GPP TS 33.501, *Security architecture and procedures for 5G System*. <https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3169>
- ENISA, *ENISA Threat Landscape for 5G Networks*, 2019. <https://www.enisa.europa.eu/publications/enisa-threat-landscape-for-5g-networks>
