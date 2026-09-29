---
title: "Web Security: Bot Defense"
layer: guide
audience: [agent, human]
stage: stable
---

# Web Security: Bot Defense

*Bad bots are more than a fifth of internet traffic: scraping, carding, credential stuffing, ad fraud, DDoS. The defense is classification and management, not prohibition.*

---

## The enemy

A **botnet** is a distributed network of malware-infected machines,
not willing participants. Bad bots account for **more than one-fifth
of all internet traffic**, and the harm is concrete:

| Attack | What it does |
|--------|--------------|
| **Carding** | Brute-force card validation against checkout |
| **Credential stuffing** | Stolen credential lists replayed at login |
| **Price scraping / inventory fraud** | Bots hold items in carts, harvest prices |
| **Click / ad fraud** | Fake clicks on ads and affiliate links |
| **DDoS** | Distributed denial of service — the availability attack |
| **Extortion** | Ransom-DDoS: pay, or the attack continues |

Not all bots are malicious — search crawlers, uptime monitors, price
comparison bots are legitimate — so the defense is **classification
and policy**, not a ban on automation.

---

## The defense shape

1. **Classify** — separate human from bot, and good bot from bad:
   behaviour (mouse, timing, navigation), reputation (IP and UA
   databases), and challenges (CAPTCHA, JS proof-of-work) for the
   uncertain middle.
2. **Manage, per policy** — allow the good bots (crawlers),
   rate-limit the grey, block the bad. The policy is the product: a
   shop wants Googlebot through and carding bots dead.
3. **Protect the endpoints that matter** — login (stuffing),
   checkout (carding), and the application as a whole (DDoS at the
   edge, upstream of the origin).

---

## Where the tools sit

| Layer | Tool class |
|-------|------------|
| Edge / CDN | DDoS absorption, rate limiting, geo policies |
| Application | **WAF** — request inspection at the app layer; bot-management rules |
| Identity | Login protection: risk scoring, MFA escalation |

The WAF is the centre of the web bot defense: it sees every request,
holds the policies, and feeds the signals (rate, reputation,
challenge) into one decision per request.

## Rules of thumb

- **Defend login and checkout first.** The bot's economy targets
  exactly those two; everything else is secondary.
- **Measure bot share.** You cannot manage what is not classified;
  bot-share dashboards are the first dashboard.
- **Challenges are a tax on humans too.** Use them for the uncertain
  middle, not as the default — every CAPTCHA costs real users.
- **DDoS defense lives at the edge.** Absorb upstream of the origin;
  a WAF on the origin absorbs nothing.

## Why it matters

Any public mesh endpoint — a station API, a web app, a checkout —
inherits the bot economy the moment it is reachable. Bot defense is
the practice that keeps automated abuse a policy problem instead of
an availability problem.
