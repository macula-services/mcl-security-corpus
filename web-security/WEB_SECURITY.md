---
title: "Web Security: The Classic Vulnerabilities"
layer: guide
audience: [agent, human]
stage: stable
---

# Web Security: The Classic Vulnerabilities

*Injection, XSS, CSRF, sessions. The classes are stable because the browser's trust model is stable — the defenses are stable for the same reason.*

---

## SQL injection

Untrusted input concatenated into a query becomes code. The fix is
not escaping — it is **parameterized queries**: the query structure and
the data never share a channel.

```
-- never:
"SELECT * FROM users WHERE id = " + user_input
-- always:
prepare("SELECT * FROM users WHERE id = ?", [user_input])
```

The rule: **never build queries by string concatenation, ever.** Every
other mitigation is a bandage on the violation of that rule.

---

## Cross-site scripting (XSS)

Foreign script executing in your page, in three flavors:

| Flavor | Where the payload lives | Defense |
|--------|------------------------|---------|
| **Stored** | Persisted (a comment, a profile) and served to everyone | Encode on **output** |
| **Reflected** | Echoed back from the request (a search result) | Encode on output; never reflect input raw |
| **DOM-based** | Executed client-side from the page's own data | Encode at the sink; audit JS data flow |

The defense in one sentence: **encode output for its context** — HTML
context, attribute context, JavaScript context each need their own
encoding. Tiny scripts in your page can still read credit card details
as a user types them; XSS is the gateway to everything else.

---

## Cross-site request forgery (CSRF)

The browser sends the user's session cookie with *every* request to
your domain — including requests the attacker's page crafts. An
attacker's page makes the victim's browser POST a transfer; the cookie
authenticates it.

Defenses:

1. **Anti-CSRF token** — a per-session secret the attacker cannot read
   (same-origin policy), required on state-changing requests.
2. **SameSite cookies** — the browser refuses to send the cookie on
   cross-site requests.
3. The structural fix: **GET must never change state.** Any
   state-changing endpoint reachable by GET is CSRF-vulnerable by
   construction.

---

## Sessions

The server hands the browser a session id (a cookie); the browser
returns it with each request; that is the whole session. Two rules:

- **The session id is a credential.** Protect it like a password:
  HTTPS-only, HttpOnly (unreadable to script), no logging it.
- **Fixation**: if the session id does not change at login, the
  attacker who planted one inherits the authenticated session. Rotate
  the id on privilege change.

---

## The trust model, stated

The browser will: send cookies everywhere, execute script it is
handed, and submit requests on the user's behalf. Every defense above
exists because of one of those three behaviours. When a new web
technology appears, ask which of the three it inherits — the
vulnerability class comes with it.

## Why it matters

These four classes are most of web security. A service that
parameterizes its queries, encodes its output, tokens its
state-changers, and treats sessions as credentials is already ahead of
most of the internet — and each fix is a one-line rule, not a product.
