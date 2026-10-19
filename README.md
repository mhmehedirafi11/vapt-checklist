# VAPT Checklist & Reporting Pack

Practical, field-tested checklists and reporting templates for **Web Application** and **Network / Infrastructure**
penetration testing (VAPT). Built from hands-on lab work and real assessments, and released so other testers can
standardise their methodology instead of starting from a blank page.

> Intended for **authorised security testing only**. Always obtain written permission and a defined scope before
> testing any system you do not own.

---

## What's inside

| File | Purpose |
|:--|:--|
| [`web-application-vapt-checklist.md`](web-application-vapt-checklist.md) | End-to-end checklist for web app & API testing, mapped to the OWASP Top 10 |
| [`network-infrastructure-vapt-checklist.md`](network-infrastructure-vapt-checklist.md) | Host discovery, service enumeration, exploitation and post-exploitation checks |
| [`reporting-template.md`](reporting-template.md) | Finding write-up template with severity, CVSS, evidence and remediation guidance |

---

## How I use it

1. **Scoping** – confirm written authorisation, targets, testing windows and out-of-scope assets.
2. **Reconnaissance** – passive first (OSINT, certificate transparency, public DNS), then active discovery.
3. **Enumeration** – walk the relevant checklist top to bottom; mark `N/A`, `Pass`, `Fail` or `Review`.
4. **Exploitation** – validate findings, capture minimal reproducible proof (never destructive).
5. **Reporting** – one entry per finding using the template, ordered by risk, each with a practical fix.
6. **Retest** – verify remediation and note the residual risk.

---

## Severity guide

| Rating | CVSS | Typical impact |
|:--|:--|:--|
| Critical | 9.0 – 10.0 | Pre-auth RCE, full database or infrastructure compromise |
| High | 7.0 – 8.9 | Privilege escalation, auth bypass, mass sensitive data exposure |
| Medium | 4.0 – 6.9 | Stored XSS, IDOR on sensitive records, weak crypto in transit |
| Low | 0.1 – 3.9 | Information disclosure, missing hardening headers, verbose errors |
| Info | — | Observations and best-practice notes with no direct impact |

---

## Tooling referenced

Burp Suite · Nmap · Metasploit · sqlmap · ffuf · Amass · Shodan · theHarvester · Wireshark · Kali Linux

---

## Contributing

Issues and pull requests are welcome — if a check is missing or unclear, open an issue with the context and a
suggested wording.

## License

[MIT](LICENSE) © 2026 Mehedi Rafi

## Contact

- GitHub: [@mhmehedirafi11](https://github.com/mhmehedirafi11)
- LinkedIn: [mehedi-rafi](https://www.linkedin.com/in/mehedi-rafi)
