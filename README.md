# Red Team Reconnaissance Playbook for a Fintech App
### Individual Contribution — Member 4

## Project
This repository contains my individual contribution to the group project **"Red Team Reconnaissance Playbook for a Fintech App"** — a portfolio-quality reconnaissance exercise modeled on real red-team client work.

## My Role
**Member 4 — Individual Contributor**

## Lab Environment
- **Host OS:** Windows
- **Virtualization:** Oracle VirtualBox
- **Attacker machine:** Kali Linux VM
  - Host-Only Adapter (host connectivity)
  - NAT Adapter (internet access, added during setup)
- **Target application:** [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — an intentionally vulnerable app maintained by OWASP for legal security training
- **Deployment method:** Docker (`bkimminich/juice-shop`)
- **Target address:** `127.0.0.1:3000` (local container, port-mapped)

## Tool Used
**Metasploit Framework v6.4.135-dev** — specifically the `auxiliary/scanner/http/http_version` module, supplemented with `curl -I` and `curl` for direct HTTP header and robots.txt inspection.

## What Was Tested
Basic HTTP-layer reconnaissance against the Juice Shop target:
- Server banner/version detection via Metasploit
- Full HTTP response header inspection via curl
- robots.txt review for disclosed internal paths

No exploitation was attempted. This was a reconnaissance-only exercise.

## Findings

| ID | Finding | Severity |
|----|---------|----------|
| F-01 | Overly Permissive CORS Policy (`Access-Control-Allow-Origin: *`) | Medium |
| F-02 | Information Disclosure via Custom HTTP Header (`X-Recruiting: /#/jobs`) | Low |
| F-03 | Missing Content-Security-Policy Header | Low |
| F-04 | Internal Path Disclosure via robots.txt (`Disallow: /ftp`) | Low |

Full details — description, evidence, impact, severity reasoning, and remediation for each finding — are in [`Red_Team_Recon_MiniReport_Member4.pdf`](./Red_Team_Recon_MiniReport_Member4.pdf).

## Remediation Summary
- Replace the wildcard CORS policy with an explicit allow-list of trusted origins.
- Remove non-standard, informational headers (e.g. `X-Recruiting`) from production responses.
- Implement a Content-Security-Policy header for browser-level XSS defense-in-depth.
- Do not use robots.txt to hide sensitive/internal paths; enforce proper authentication/authorization instead.

## Evidence
Screenshots supporting each finding are included in this repository:
- `_docker_ps.png` — Juice Shop container running and port-mapped
- `_msf_scan.png` — Metasploit scan executed against the target
- `_curl_headers.png` — Raw HTTP headers (primary evidence for F-01, F-02, and F-03)
- `_robots_txt.png` — robots.txt output disclosing the internal /ftp path (evidence for F-04)

## Authorization & Scope
All testing was performed exclusively against a self-hosted, local instance of OWASP Juice Shop running in an isolated Kali Linux VM under my own control. **No real, live, third-party, or production systems were accessed, scanned, or tested at any point.**

## Files in This Repository
- `Red_Team_Recon_MiniReport_Member4.pdf` — full professional mini-report
- `Red_Team_Recon_MiniReport_Member4.docx` — editable version of the report
- `_docker_ps.png`
- `_msf_scan.png`
- `_curl_headers.png`
- `_robots_txt.png`
- `README.md` — this file
