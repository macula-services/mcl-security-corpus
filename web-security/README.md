---
title: Web Security
layer: index
audience: [agent, human]
stage: stable
---

# Web Security

*The web's classic vulnerabilities and their defenses.*

---

## Notes

| Note | Covers |
|------|--------|
| [WEB_SECURITY](WEB_SECURITY.md) | Injection, XSS (stored/reflected/DOM), CSRF, sessions |

## One-page summary

The web's vulnerability classes are stable because the browser's trust
model is stable: input crosses trust boundaries unvalidated
(**injection**), pages execute foreign script (**XSS**, in three
flavors), requests ride the user's authenticated session (**CSRF**),
and everything hangs on the **session cookie**. Defense is the mirror:
parameterize, encode, tokenize, and treat the session as a credential.
