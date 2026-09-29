---
title: "Web Security: The Classic Vulnerabilities"
layer: guide
audience: [agent, human]
stage: stable
---

# Web Security: The Classic Vulnerabilities

*Injection, XSS, CSRF and session flaws keep recurring because they follow from how browsers and servers mix code, data and ambient credentials. The defenses are well understood and mostly structural.*

---

## Why these four

Browsers do three things by design: they attach stored cookies to
requests for a site, they run script delivered as part of a page, and
they send requests that any page can trigger. Servers, meanwhile, often
assemble commands (SQL, HTML, shell) out of strings. Each classic flaw
below sits where one of those behaviours meets untrusted input. The
OWASP Top 10 still carries them under Injection, Broken Access Control
and Authentication Failures.

---

## Injection (SQL as the model case)

**What goes wrong.** The application builds a query by pasting user
input into the query text, so the database cannot tell which part the
developer wrote and which part the user supplied. Input can then change
the query's logic.

**Prevention.** Keep code and data in separate channels:

- Use parameterized queries (prepared statements) or an ORM that binds
  values, for every query that includes external data.
- Where a value cannot be bound (a table or column name, a sort
  direction), map it against an allow-list of known values.
- Run the application's database account with least privilege, so a
  flaw that slips through has limited reach.
- Treat escaping as a last resort; OWASP explicitly discourages relying
  on it.

**Detection.** Database errors surfacing in responses, unusual query
shapes in database audit logs, and WAF or application logs showing
quote and comment characters in parameters that should be numeric.

The same reasoning covers OS command, LDAP and template injection:
prefer APIs that take arguments as data over building command strings.

---

## Cross-site scripting (XSS)

**What goes wrong.** Untrusted data is placed into a page in a way the
browser interprets as markup or script, so script from someone else runs
with the site's origin and can act as the user.

| Variant | Where the untrusted data comes from | Where to fix it |
|---------|------------------------------------|-----------------|
| Stored | Saved content served to other users (profiles, comments) | Encode when rendering |
| Reflected | Parameters of the current request echoed in the response | Encode when rendering; do not echo raw input |
| DOM-based | Client-side code moves data into a dangerous sink (for example `innerHTML`) | Use safe sinks such as `textContent`; review client data flow |

**Prevention.**

- Encode output for the exact context it lands in (HTML body, HTML
  attribute, JavaScript, CSS, URL); each context has different rules.
- Prefer frameworks that auto-escape, and treat their escape hatches as
  review items.
- When users are allowed to submit HTML, run it through a maintained
  sanitizer rather than a home-made filter.
- Add a Content Security Policy as a second layer. It limits damage; it
  does not replace encoding.
- Mark session cookies HttpOnly so script cannot read them.

**Detection.** CSP violation reports, stored content containing markup
where plain text is expected, and scanner findings in CI.

---

## Cross-site request forgery (CSRF)

**What goes wrong.** Because the browser attaches the user's cookies to
requests for the site, a page on another origin can cause the user's
browser to send a state-changing request that the server accepts as
genuine.

**Prevention.**

- Use the framework's built-in CSRF protection if it has one.
- Require an unpredictable per-session token on state-changing requests
  (synchronizer token), or a signed double-submit cookie for stateless
  designs.
- Set `SameSite` on session cookies (`Lax` or `Strict`) as defense in
  depth; `Lax` still allows top-level navigations, so it is not a full
  fix by itself.
- Verify `Origin` (or Fetch Metadata headers) on unsafe methods.
- Never change state on GET.

Note that any XSS on the same origin defeats CSRF defenses, so the two
must be fixed together.

---

## Session management

**What goes wrong.** The session identifier is a bearer credential:
whoever presents it is the user. It can be stolen (sniffed, leaked in
logs or URLs, read by script) or fixed in advance by an attacker if the
application keeps the same identifier across login.

**Prevention.**

- Generate identifiers with a cryptographically secure random source.
- Set cookie attributes `Secure`, `HttpOnly` and an appropriate
  `SameSite`; never put the identifier in a URL.
- Regenerate the identifier on login and on any privilege change
  (prevents fixation).
- Enforce idle and absolute timeouts, and invalidate server-side on
  logout.

**Detection.** One session identifier seen from widely different client
fingerprints or networks, sessions active long past expected lifetime.

---

## Pitfalls

- Filtering "bad characters" on input instead of fixing how output and
  queries are built.
- Assuming a WAF or CSP makes the underlying flaw acceptable.
- Protecting the main form and forgetting the JSON API that performs
  the same action.

## Related notes

Thread these classes into every [threat model](../secure-design/SECURE_DESIGN.md);
[BOT_DEFENSE](BOT_DEFENSE.md) covers abuse of features that work as
designed; [API_TESTING](../pentest/API_TESTING.md) covers how the same
classes appear in APIs.

## Sources

- *Web Security for Developers: Real Threats, Practical Defense*, Malcolm McDonald, No Starch Press, 2020. https://nostarch.com/websecurity
- OWASP Top 10:2025. https://top10.owasp.org/2025/
- OWASP SQL Injection Prevention Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
- OWASP Cross Site Scripting Prevention Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
- OWASP Cross-Site Request Forgery Prevention Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html
- OWASP Session Management Cheat Sheet. https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
