# Lab 02 — Diagnose a Broken Certificate Chain

## Incident Summary

**Target System:** Radiology imaging platform (simulated via incomplete-chain.badssl.com)

**Reported Behavior:** TLS failure after certificate renewal — connection fails even though certificate appears valid

**Diagnostic Scope:** PKI Diagnostic Framework — all 4 steps

## Diagnostic Steps

**Step 1 — Retrieve:**  
I used OpenSSL to connect to `incomplete-chain.badssl.com:443` and retrieve the server certificate. The connection succeeded, but only one certificate was returned. I saved this certificate as `leaf_cert.pem` for further inspection.

**Step 2 — Parse:**  
I reviewed the certificate details using OpenSSL. The certificate showed a valid subject and issuer, indicating that it was properly issued. However, the issuer listed in the certificate was not included in the server response.

**Step 3 — Validate the Chain:**  
The server provided only one certificate, which means the intermediate certificate was missing. Because the issuer certificate was not provided, the client could not build a complete trust chain. This resulted in a chain validation failure.

**Step 4 — Check Revocation and Trust:**  
The certificate itself is not expired and does not show revocation issues. However, the trust cannot be established because the chain is incomplete. Without the intermediate certificate, the client cannot verify the certificate’s authenticity.

## Evidence

- **Leaf certificate Subject:** CN=*.badssl.com  
- **Issuer CN (missing intermediate):** COMODO RSA Domain Validation Secure Server CA  
- **Number of certificates the server sent:** 1  
- **Chain status:** Incomplete  
- **Issuer certificate present:** No  
- **Verification Result:** Unable to build certificate chain (missing intermediate)  
- **Is the root CA trusted by your system?** Yes  

## Root Cause

This is a server configuration issue. The server failed to provide the required intermediate certificate, resulting in an incomplete certificate chain. Even though the certificate itself is valid, the missing intermediate prevents clients from establishing trust.

## Remediation

1. Install the correct intermediate certificate on the server  
2. Configure the server to present the full certificate chain (leaf + intermediate)  
3. Restart the service and verify TLS connectivity  

## Key Findings

The failure was not caused by an expired or revoked certificate, but by an incomplete certificate chain. This highlights the importance of correctly configuring servers to provide all necessary certificates during the TLS handshake.

## Challenges / Troubleshooting

I initially expected multiple certificate blocks but observed that only one certificate was returned. This helped confirm that the issue was a missing intermediate certificate rather than another type of TLS failure.

## Artifacts

- leaf_cert_w5.pem
