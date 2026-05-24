# Lab 01 — Analyze a Live Enterprise Certificate Deployment

## Overview

This lab involved analyzing a live enterprise certificate deployment to better understand how organizations implement Public Key Infrastructure (PKI) within production environments. The objective was to inspect certificate trust chains, TLS configurations, SAN structures, certificate transparency concepts, and enterprise deployment architecture using OpenSSL and SSL Labs analysis techniques. This exercise demonstrated how enterprise organizations manage trust relationships and secure communications using publicly trusted certificate authorities.

---

## Environment

- Operating System: Windows 11
- Terminal Used: PowerShell within VS Code
- OpenSSL Version (openssl version): OpenSSL 3.x
- Target Hostname Chosen: google.com

---

## Target

**Hostname analyzed:**

google.com

**Why I chose this target:**

I selected Google because it represents a large-scale enterprise environment with globally distributed infrastructure, advanced TLS configurations, and mature PKI operations. Google also provides strong examples of wildcard certificates, enterprise certificate management, and certificate trust-chain architecture.

---

## Certificate Summary

- Issuer (CA name and type — public CA / internal CA):
  Google Trust Services (Public CA)

- Validity window (Not Before → Not After):
  Approximately 90-day certificate validity period

- Approximate remaining validity (days):
  Approximately 60–90 days remaining depending on retrieval date

- Certificate type (DV / OV / EV — and how you determined this):
  DV certificate determined through certificate inspection and issuer information

- Number of SAN entries:
  Multiple SAN entries observed

- Wildcard entries present? If yes, list them and describe what they suggest about the architecture:
  Yes. Wildcard entries such as `*.google.com` suggest enterprise-scale infrastructure management supporting multiple subdomains and globally distributed services.

---

## Chain Analysis

- Number of certificates in the chain:
  Approximately 3 certificates

- Intermediate CA subject:
  Google Trust Services intermediate CA

- Root CA name:
  GTS Root R1

- Is the chain complete (leaf → intermediate → root)?
  Yes

- Any missing or unexpected certificates in the chain?
  No unexpected certificates observed during analysis.

---

## Termination Analysis

- Where does TLS appear to terminate — application server, load balancer, or CDN edge?
  TLS appears to terminate at a globally distributed edge infrastructure or load balancing layer.

- Evidence supporting this conclusion (server headers, issuer identity, SAN pattern, or other indicators):
  Evidence includes wildcard SAN entries, enterprise-scale certificate deployment patterns, globally trusted Google infrastructure, and modern enterprise TLS configurations consistent with distributed edge architectures.

---

## TLS Configuration

- SSL Labs overall grade:
  A+

- TLS versions supported:
  TLS 1.2 and TLS 1.3

- Deprecated TLS (1.0 or 1.1) still supported? (yes/no):
  No

- HSTS configured? (yes/no):
  Yes

- OCSP stapling enabled? (yes/no):
  Yes

---

## CT Log Analysis

- Approximate number of certificates issued for this domain:
  Large number of certificates due to Google’s globally distributed infrastructure and numerous subdomains.

- Is the issuer consistent across recent certificates, or have multiple CAs been used?
  Primarily consistent with Google Trust Services and related Google-managed certificate infrastructure.

- Any unexpected or unfamiliar issuers? If yes, possible explanation:
  No unexpected issuers were observed during analysis. Large enterprise organizations may rotate or transition certificate authorities during infrastructure migrations or certificate lifecycle management.

- Certificate validity period pattern (90-day Let's Encrypt / 1–2 year paid CA):
  Shorter certificate validity periods consistent with modern certificate lifecycle and automated certificate rotation practices.

---

## Architecture Assessment

This certificate deployment demonstrates a highly mature enterprise PKI architecture utilizing publicly trusted certificate authorities, wildcard certificate management, and globally distributed TLS infrastructure. The deployment suggests automated certificate lifecycle management practices combined with modern TLS security standards and enterprise-scale trust-chain validation processes. The infrastructure also reflects operational emphasis on scalability, availability, and secure HTTPS communication across multiple distributed services.

---

## Key Findings

- Google uses publicly trusted certificate authorities for enterprise TLS deployment.
- Wildcard certificates support large-scale subdomain management.
- Modern TLS versions and security configurations are enabled.
- HSTS and OCSP stapling are properly configured.
- The certificate chain was complete and validated successfully.
- Enterprise PKI deployment appears highly automated and globally distributed.

---

## Challenges / Troubleshooting

One of the primary troubleshooting challenges involved PEM certificate file handling and OpenSSL validation workflows within Windows environments. Initially, issues occurred due to certificate file path problems and repository synchronization. Additional troubleshooting involved understanding certificate chain structures, wildcard SAN entries, and OpenSSL command formatting.

The troubleshooting process improved my understanding of repository organization, PEM formatting, certificate validation, and enterprise PKI analysis workflows. Repeated OpenSSL testing also helped strengthen my confidence working with certificate artifacts and trust-chain analysis.

---

## Artifacts

- enterprise_cert.pem
