# Network & Infrastructure — VAPT Checklist

Checklist for internal and external network penetration tests: discovery, enumeration, exploitation,
post-exploitation and hardening review.

**Status legend:** `[ ]` to test · `Pass` · `Fail` · `N/A` · `Review`

---

## 0. Pre-engagement

- [ ] Scope confirmed: CIDR ranges, excluded hosts, VPN/access method, testing window
- [ ] Written authorisation and emergency contact available on hand
- [ ] Fragile systems (legacy, ICS/OT, medical, PBX) identified and excluded or scheduled
- [ ] Credentials supplied for authenticated testing (domain user, local admin if in scope)

---

## 1. Discovery & host enumeration

- [ ] Live host discovery (ICMP, ARP in local segment, TCP SYN sweep on common ports)
- [ ] Reverse DNS lookups and hostname correlation
- [ ] Full TCP port scan plus targeted UDP scan (161, 500, 623, 1900, 5353)
- [ ] Service/version detection and OS fingerprinting (`-sV -O`)
- [ ] Default and NSE scripts for safe discovery (`--script default,safe`)

## 2. Service-specific enumeration

- [ ] SMB: null sessions, shares, signing, SMBv1, user enumeration (`enum4linux-ng`, `smbmap`)
- [ ] RDP: NLA, self-signed certificate exposure, BlueKeep-class checks
- [ ] SSH: supported algorithms, weak/exposed keys, default credentials, version CVEs
- [ ] LDAP/AD: anonymous binds, LDAP signing, password policy, Kerberos (`kerbrute`)
- [ ] HTTP(S): virtual hosts, default pages, exposed management consoles, server headers
- [ ] Database services (1433, 1521, 3306, 5432, 27017, 6379, 9200): anonymous access, weak auth
- [ ] Mail (25/110/143/465/587/993/995): open relay, user enumeration (`VRFY`, `EXPN`)
- [ ] SNMP: public/private community strings, MIB walking for user and interface data
- [ ] IPMI (623): cipher zero, hash disclosure for offline cracking
- [ ] Printers, VoIP, CCTV and NAS devices on the same segment

## 3. Vulnerability identification

- [ ] Missing security patches correlated with the detected service versions
- [ ] Weak TLS: SSLv2/3, TLS 1.0/1.1, RC4/3DES, expired or mis-scoped certificates
- [ ] Credentialed scan for missing updates and configuration drift
- [ ] Unauthenticated network access to administrative interfaces
- [ ] Firewall/segmentation review: can a low-value host reach high-value subnets?
- [ ] Wireless (if in scope): open/WEP/WPS, weak PSK, rogue AP detection

## 4. Exploitation (authorised, non-destructive)

- [ ] Validate each candidate vulnerability with the minimum-impact proof of concept
- [ ] Relayed/coerced authentication (Responder, NTLM relay) where permitted
- [ ] Password spraying against exposed authentication services (respect lockout policy)
- [ ] Known-exploitable CVEs verified against the actual patch level
- [ ] Document exact payload, timestamp and effect for every successful exploit

## 5. Post-exploitation & lateral movement

- [ ] Local privilege escalation paths (misconfigured services, unquoted paths, SUID/SGID)
- [ ] Credential discovery: SAM/LSA, `/etc/shadow`, config files, browser stores
- [ ] Lateral movement to demonstrate real business impact — stop at agreed boundaries
- [ ] Domain dominance impact: Kerberoasting, AS-REP roasting, delegation abuse (report only)
- [ ] Persistence mechanisms are **not** installed unless explicitly authorised

## 6. Hardening review (report recommendations)

- [ ] Host firewalls enabled with least-privilege rules; unnecessary services stopped
- [ ] Centralised logging and alerting for authentication and privilege events
- [ ] Patch management process and cadence in place
- [ ] Network segmentation between user, server and management zones
- [ ] Strong authentication for administrative access; no shared local admin passwords (LAPS)
- [ ] Backups tested and stored offline/immutable
- [ ] Asset inventory current, so unknown devices cannot hide on the network

---

## Evidence to capture per finding

- Affected host (IP/hostname), port and service version
- Exact command or tool invocation used
- Raw output proving the issue (never edit screenshots)
- Business impact in plain language
- Remediation steps, specific to the platform

## Sign-off

- [ ] Findings de-duplicated and grouped by root cause
- [ ] Severity agreed with the client using CVSS plus business context
- [ ] Retest scheduled and residual risk documented
- [ ] All captured credentials and test artefacts securely destroyed at engagement close
