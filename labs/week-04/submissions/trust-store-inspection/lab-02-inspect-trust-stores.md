# Lab 02 — Inspect Your Trust Store

## Overview
This lab focused on examining the trusted root certificate authorities
installed on my system. The goal was to understand how operating systems
and browsers establish trust using pre-installed root CAs and how
certificate chains are validated.

---

## Environment
- Operating System: Windows 11
- Terminal Used: N/A (GUI + certificate manager)
- OpenSSL Version: OpenSSL 3.x (installed previously)

---

## Steps Performed
1. Opened the Windows Certificate Manager (certmgr.msc)
2. Navigated to Trusted Root Certification Authorities → Certificates
3. Reviewed the list of installed root CAs
4. Selected a DigiCert root certificate and inspected:
   - Details tab
   - Certification Path tab

---

## Results
- The system contained a large number of trusted root CAs (dozens pre-installed)
- Example root CA inspected:
  - Name: DigiCert Trusted Root G4
  - Subject: CN=DigiCert Trusted Root G4
  - Expiration: Long-term validity (2030+ range depending on cert)
- The certification path confirmed the certificate is trusted
  and acts as a root authority
- No verification errors were present, indicating a valid trust store

---

## Key Findings
- Operating systems come with many pre-installed trusted root CAs
- These root certificates act as trust anchors
- Trust is inherited through certificate chains (root → intermediate → leaf)
- Users do not manually approve most certificates because trust is automatic

---

## Explanation
Browsers trust certificates from websites because they rely on the system's
trusted root CA store. When visiting a site, the server presents a certificate
chain, and the browser verifies that chain back to a trusted root CA.

If an enterprise root CA is not installed on a system, any certificates issued
by that CA will not be trusted, and users will receive security warnings.

One surprising finding was how many root CAs are already pre-installed on the
system, which highlights how much trust is delegated to external authorities.

---

## Challenges / Troubleshooting
- Initial confusion about where to locate the trust store on Windows
- Needed to explore certificate properties to understand fields like
  key usage and certificate policies
- Learned that GUI tools can provide the same insights as OpenSSL commands

---

## Artifacts
- assets/screenshots/week-04/trust-store-list.png
- assets/screenshots/week-04/digicert-details.png
- assets/screenshots/week-04/digicert-cert-path.png
