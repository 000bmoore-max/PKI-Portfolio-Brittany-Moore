# Lab 01 — Diagnose an Expired Certificate

## Incident Summary

**Target System:** portal.metrogeneral.org (simulated via expired.badssl.com)

**Reported Behavior:** TLS failure — patients seeing security warnings when accessing the appointment portal

**Diagnostic Scope:** PKI Diagnostic Framework — all 4 steps

## Diagnostic Steps

**Step 1 — Retrieve:**  
I used OpenSSL to connect to `expired.badssl.com:443` and retrieve the server certificate. The connection completed, but the verification output showed that the certificate was expired. I then saved the certificate as `expired_cert.pem` for further analysis.

**Step 2 — Parse:**  
I used OpenSSL to parse the certificate and examine its details, including subject, issuer, and validity dates. This allowed me to confirm that the certificate had already passed its expiration date. The parsing step clearly showed that the certificate was no longer within its valid time range.

**Step 3 — Validate the Chain:**  
I reviewed the certificate chain presented by the server. The chain was complete, meaning the server provided the necessary intermediate certificate. However, even with a valid chain, the expired leaf certificate caused the validation to fail.

**Step 4 — Check Revocation and Trust:**  
I checked the verification result from OpenSSL. The output showed `Verify return code: 10 (certificate has expired)`, which means the certificate is no longer trusted. This confirms the issue is expiration rather than revocation or trust chain failure.

## Evidence

- **Not Before date:** Apr 9 00:00:00 2015 GMT  
- **Not After date:** Apr 12 23:59:59 2015 GMT  
- **Days since expiration:** Several years (certificate expired long ago)  
- **Subject (entity the certificate was issued to):** CN=*.badssl.com  
- **Issuer:** COMODO RSA Domain Validation Secure Server CA  
- **Chain status (complete / incomplete):** Complete  
- **OCSP URL present? (yes/no):** Yes  
- **Verification Result:** Verify return code: 10 (certificate has expired)

## Root Cause

The TLS failure was caused by an expired certificate. This is a certificate issue, not a chain or configuration problem. Because the certificate is outside its valid date range, clients no longer trust it and display security warnings.

## Remediation

Step-by-step path to resolve this incident:

1. Generate a new certificate signing request (CSR) for the server.  
2. Obtain a renewed certificate from a trusted Certificate Authority.  
3. Install the new certificate and verify the TLS connection works without errors.

## Key Findings

The certificate chain was valid, but the expired leaf certificate caused the TLS handshake to fail. This demonstrates that certificate validity dates are critical for maintaining trust, even when the chain itself is properly configured.

## Challenges / Troubleshooting

I encountered issues with file paths and extensions when saving the certificate file. After correcting the file location and ensuring the `.pem` extension was used, I was able to successfully analyze the certificate and identify the expiration issue.

## Artifacts

- expired_cert.pem
