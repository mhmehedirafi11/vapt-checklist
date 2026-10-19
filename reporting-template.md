# Finding Report Template

Copy the block below once per finding. Keep one finding per issue — if the same root cause affects several
endpoints, one finding with a list of affected URLs is clearer than five near-identical entries.

---

## Finding 01 — `<Short, specific title, e.g. Unauthenticated SQL Injection in /api/v1/search>`

| Field | Value |
|:--|:--|
| **Severity** | Critical / High / Medium / Low / Informational |
| **CVSS v3.1** | `9.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)` |
| **CWE** | `CWE-89 Improper Neutralisation of Special Elements used in an SQL Command` |
| **OWASP Top 10** | `A03:2021 – Injection` |
| **Affected asset** | `https://app.example.com/api/v1/search` |
| **Component / parameter** | `q` (query string) |
| **Status** | Open / Fixed / Retest passed / Accepted risk |

### Description

Two or three sentences explaining what the issue is and why it exists. Written so a non-security stakeholder
understands it.

### Business impact

What an attacker gains and what it costs the business — data exposure, downtime, regulatory exposure
(GDPR/PII), financial loss, or reputational damage. Be concrete, avoid hyperbole.

### Steps to reproduce

1. Authenticate as a standard user (or note "no authentication required").
2. Send the following request:

```http
POST /api/v1/search HTTP/1.1
Host: app.example.com
Content-Type: application/json

{"q":"' OR 1=1-- -"}
```

3. Observe the response:

```json
{"error":"SQLSTATE[42000]: Syntax error ... near 'OR 1=1'"}
```

4. Confirm the injection with a time-based payload (`' AND SLEEP(5)-- -`) — response delayed by ~5 seconds.

### Evidence

- `evidence/01-sqli-error.png`
- `evidence/01-sqli-timing.txt` (raw request/response)
- Burp Suite project file: `evidence/01-burp-project.burp`

### Affected instances

| URL | Parameter | Verified |
|:--|:--|:--|
| `/api/v1/search` | `q` | Yes |
| `/api/v1/products` | `filter` | Yes |

### Remediation

Primary fix first, then supporting measures:

1. Replace string-concatenated SQL with **parameterised queries** / prepared statements in the ORM.
2. Enforce server-side input validation and allow-list for search parameters.
3. Apply least privilege to the database account used by the application (no `DROP`, no cross-schema reads).
4. Enable WAF rules for SQL injection patterns as a defence-in-depth layer, not as the fix.
5. Add regression tests covering injection payloads to the CI pipeline.

### References

- OWASP SQL Injection Prevention Cheat Sheet
- CWE-89 — <https://cwe.mitre.org/data/definitions/89.html>

---

## Executive summary template (start of report)

> Between `<start>` and `<end>`, an authorised penetration test was performed against `<n>` targets covering
> `<web apps / APIs / internal network>`. The assessment identified **`<x>` critical** and **`<y>` high**
> severity issues, of which `<n>` would allow an unauthenticated attacker to `<business impact>`.
> The most significant weakness is `<root cause theme>`, affecting `<scope>`. Addressing the findings in the
> remediation roadmap, starting with the critical items, is expected to materially reduce the risk of
> compromise.

### Risk distribution

| Severity | Count |
|:--|--:|
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |
| Informational | 0 |

### Remediation roadmap

| Priority | Findings | Suggested timeframe |
|:--|:--|:--|
| 1 | Critical | 0 – 7 days |
| 2 | High | 7 – 30 days |
| 3 | Medium | 30 – 90 days |
| 4 | Low / Informational | Next maintenance window |
