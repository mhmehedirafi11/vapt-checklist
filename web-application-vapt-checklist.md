# Web Application & API — VAPT Checklist

Scope-driven checklist for testing web applications and REST/GraphQL APIs. Entries are grouped by OWASP Top 10
(2021) category, followed by API-specific and hardening checks.

**Status legend:** `[ ]` to test · `Pass` · `Fail` · `N/A` · `Review`

---

## 0. Pre-engagement

- [ ] Written authorisation collected (scope, IPs/domains, testing window, emergency contact)
- [ ] Rules of engagement agreed (no DoS, no destructive payloads, data handling)
- [ ] Test accounts provisioned for each role (anonymous, user, admin)
- [ ] Out-of-scope assets and third-party services explicitly listed
- [ ] Backup / rollback plan confirmed with the client

---

## A01 — Broken Access Control

- [ ] Horizontal privilege escalation: access another user's objects by ID (IDOR)
- [ ] Vertical privilege escalation: reach admin-only functions as a low-privilege user
- [ ] Forced browsing to unlinked endpoints, admin panels and API routes
- [ ] HTTP method tampering (`GET` → `PUT`/`DELETE`/`PATCH`) on privileged endpoints
- [ ] JWT / session claims can be modified to escalate role (tamper, `alg:none`, weak HMAC secret)
- [ ] Access-control checks enforced server-side, not only in the UI/spa router
- [ ] CORS misconfiguration allowing arbitrary origins with credentials
- [ ] Directory listing and exposed backups, `.git/`, `.env`, `*.bak`, `*.sql`

## A02 — Cryptographic Failures

- [ ] Site fully HTTPS, no mixed content; HSTS present with sane `max-age`
- [ ] TLS configuration: protocol versions, weak ciphers, certificate chain
- [ ] Passwords hashed with a modern KDF (bcrypt / argon2 / scrypt) — never MD5/SHA1
- [ ] Sensitive data never in URL query strings, logs or local storage
- [ ] Cookies: `Secure`, `HttpOnly`, `SameSite`, no sensitive values in cleartext
- [ ] Encryption keys and secrets not committed to the repository or shipped in client bundles

## A03 — Injection

- [ ] SQL injection: error-based, boolean, time-based, and out-of-band (test `ORDER BY`, headers, JSON fields)
- [ ] NoSQL / ORM injection (MongoDB operators, ORM raw query usage)
- [ ] Command injection and shell metacharacters in system-integrated parameters
- [ ] Server-side template injection (SSTI) in any user-reflected template input
- [ ] XML external entity (XXE) in file uploads, SOAP and SVG parsing
- [ ] LDAP, XPATH and mail-header injection where those backends are integrated
- [ ] Deserialisation of untrusted data (Java, PHP, Python pickle, .NET)

## A04 — Insecure Design

- [ ] Business-logic abuse: negative quantities, coupon reuse, price/role tampering
- [ ] Registration, password reset and MFA flows resistant to enumeration and bypass
- [ ] Rate limiting and anti-automation on login, OTP, search and export endpoints
- [ ] Multi-step processes (checkout, KYC, approval) cannot be skipped or replayed
- [ ] Threat model documented for high-value workflows

## A05 — Security Misconfiguration

- [ ] Default credentials and unused sample/demo accounts removed
- [ ] Verbose errors / stack traces disabled in production
- [ ] Security headers set: CSP, `X-Content-Type-Options`, `X-Frame-Options`/`frame-ancestors`, `Referrer-Policy`
- [ ] Admin interfaces and debug endpoints not publicly reachable
- [ ] Unnecessary HTTP methods, features, ports and services disabled
- [ ] Cloud storage buckets and object ACLs correctly scoped

## A06 — Vulnerable & Outdated Components

- [ ] Inventory of front-end and back-end dependencies with known CVEs
- [ ] Framework, CMS and plugin versions not exposed in headers, comments or meta tags
- [ ] Patch level of the runtime (Node, PHP, Python, JVM) and web server
- [ ] Third-party scripts/widgets reviewed (supply-chain and data-leak risk)

## A07 — Identification & Authentication Failures

- [ ] Password policy, breached-password checks and account lockout behaviour
- [ ] Username/email enumeration via login, registration, reset and timing differences
- [ ] Session fixation, session timeout, and session invalidation on logout/password change
- [ ] "Remember me" tokens are random, server-validated and revocable
- [ ] OAuth/OIDC flows: `state`, PKCE, redirect URI validation, token leakage in referrer
- [ ] MFA cannot be bypassed by direct API calls or by re-using an old OTP

## A08 — Software & Data Integrity Failures

- [ ] Client-side input trusted for security decisions (price, role, permissions)
- [ ] Unsigned/unverified software updates or auto-update channels
- [ ] Insecure deserialisation of session or cache data
- [ ] CI/CD pipeline inputs (branch names, PR titles) not injectable
- [ ] Subresource integrity (SRI) on third-party scripts

## A09 — Security Logging & Monitoring Failures

- [ ] Authentication, authorisation and input-validation failures are logged
- [ ] Logs contain no secrets, tokens or full PII
- [ ] Alerting exists for repeated auth failures, privilege changes and error spikes
- [ ] Log integrity protected from tampering by application users

## A10 — Server-Side Request Forgery (SSRF)

- [ ] URL-accepting features (webhooks, image/PDF fetch, importers) tested against internal ranges
- [ ] Redirects and DNS rebinding used to bypass allow-lists
- [ ] Access to cloud metadata endpoints (`169.254.169.254`) blocked
- [ ] Egress filtering from the application tier in place

---

## API-specific checks

- [ ] Enumerate all versions (`/v1/`, `/v2/`) and deprecated endpoints still reachable
- [ ] Broken object-level authorisation across every object ID in every endpoint
- [ ] Mass assignment: inject extra JSON fields and observe privilege/fields changes
- [ ] Excessive data exposure: compare API response fields with what the UI actually needs
- [ ] GraphQL: introspection enabled, depth/alias abuse, batching for rate-limit bypass
- [ ] Rate limiting and pagination limits on list endpoints
- [ ] Swagger/OpenAPI documentation exposed with internal routes

## Client-side checks

- [ ] Reflected, stored and DOM-based XSS, including `innerHTML` sinks and postMessage handlers
- [ ] CSRF protection on state-changing requests (token or SameSite enforcement)
- [ ] Open redirect and reverse tabnabbing (`target=_blank` without `rel=noopener`)
- [ ] Client-side secrets in JavaScript bundles, source maps and mobile app decompilation
- [ ] Content sniffing and unsafe file upload (`Content-Type`, extension, magic bytes, stored XSS)

## Authentication & transport extras

- [ ] File upload limits, allowed types and quarantine of executables
- [ ] Password reset tokens: entropy, single use, expiry, not leaked via referrer
- [ ] Email/SMS notification flows do not disclose data to unverified addresses
- [ ] Timing attacks against token comparison and user lookups

---

## Sign-off

- [ ] All findings reproduced and evidence captured (request/response pairs, screenshots)
- [ ] Each finding has a severity, an affected component and a concrete remediation
- [ ] Retest performed after fixes; residual risk documented
