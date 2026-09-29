---
title: "Web Security: Bot Defense"
layer: guide
audience: [agent, human]
stage: stable
---

# Web Security: Bot Defense

*Automated abuse uses a site's legitimate features at a scale and intent the owner never agreed to. Defense means telling good automation from bad, then applying a policy, not banning bots outright.*

---

## What the problem is

Many harmful bots do not exploit a bug. They call the login form, the
checkout, the search box or the gift-card balance page exactly as
designed, only thousands of times a minute and for someone else's
benefit. OWASP's Automated Threats project catalogues these as OAT
entries; a few that matter to most sites:

| Automated threat (OWASP OAT) | What the abuse looks like |
|------------------------------|---------------------------|
| Credential Stuffing (OAT-008) | Username and password pairs leaked elsewhere tried against your login |
| Carding (OAT-001) | Stolen card details tested with small payments to find valid ones |
| Scraping (OAT-011) | Bulk collection of content or prices |
| Denial of Inventory (OAT-021) | Items held in carts or bookings so real customers cannot buy them |
| Skewing (OAT-016) | Repeated clicks or requests that distort a metric, such as ad clicks or votes |
| Denial of Service (OAT-015) | Enough application-level requests to exhaust the service |

The traffic often comes from botnets (compromised devices whose owners
are unaware) or from residential proxy networks, which makes IP address
alone a weak signal.

Plenty of automation is wanted: search engine crawlers, uptime checks,
partner integrations, accessibility tools. Blocking all of it hurts the
business, so the goal is classification plus policy.

---

## How defense works

1. **Identify the features worth abusing.** Login, account creation,
   password reset, payment, gift-card and coupon checks, inventory
   holds, search. These are the OAT targets for your application.
2. **Collect signals per request and per session.** Request rate and
   pattern, IP and ASN reputation, TLS and HTTP client fingerprints,
   header consistency, device signals, and behaviour such as navigation
   order and timing. Verified good bots can be recognised by reverse DNS
   or published IP ranges rather than by the user-agent string, which is
   trivially set.
3. **Decide per policy.** Allow verified good bots, throttle or
   challenge the uncertain, block the clearly abusive. The policy is a
   business decision per endpoint.
4. **Respond in proportion.** Rate limits, step-up authentication,
   challenges, delayed or degraded responses, account lockout with
   notification.

For credential stuffing specifically, OWASP ranks MFA as the most
effective control, supported by checking passwords against known-breach
lists and risk-based step-up for logins from new devices or networks.

---

## Where controls sit

| Layer | Controls |
|-------|----------|
| Network edge / CDN | Volumetric DDoS absorption, coarse rate limits, geo and ASN policy |
| Application edge (WAF, bot management) | Request inspection, fingerprinting, per-endpoint rate limits, challenges |
| Application logic | Business limits (cards per account per hour, holds per session), idempotency, abuse flags |
| Identity | MFA, breached-password checks, risk scoring, user notifications |

Volumetric floods must be absorbed upstream of the origin; a WAF in
front of a saturated link cannot help. Application-layer abuse, by
contrast, is often only visible with business context, so some limits
belong in the application itself.

---

## Detection signals

- Login failure ratio rising sharply, especially spread across many
  accounts from many IPs (stuffing) rather than many tries on one
  account (brute force).
- Clusters of small-value payment authorisations and declines (carding).
- Carts or reservations created and abandoned at machine pace.
- Traffic share by classification (human, good bot, bad bot, unknown)
  tracked over time; a sudden shift is an incident signal.

## Trade-offs and pitfalls

- Challenges and CAPTCHAs burden real users and can exclude people with
  disabilities; reserve them for the uncertain middle.
- Attackers adapt to any single signal. Layer signals and review
  policies when metrics drift.
- Blocking by IP alone fails against proxy networks and punishes users
  behind shared addresses.
- Bot defense does not fix vulnerabilities; it limits abuse of working
  features. Pair it with the fixes in [WEB_SECURITY](WEB_SECURITY.md).

## Related notes

Any publicly reachable mesh endpoint inherits these threats;
[NETWORK_ATTACKS](../pentest/NETWORK_ATTACKS.md) covers botnets from the
network side, and [API_TESTING](../pentest/API_TESTING.md) covers rate
limiting and resource consumption in APIs.

## Sources

- No single book source could be identified for this note; it is based on open OWASP material.
- OWASP Automated Threats to Web Applications (OAT catalogue and handbook). https://owasp.org/www-project-automated-threats-to-web-applications/
- OWASP Bot Management and Anti-Automation Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Bot_Management_and_Anti-Automation_Cheat_Sheet.html
- OWASP Credential Stuffing Prevention Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html
