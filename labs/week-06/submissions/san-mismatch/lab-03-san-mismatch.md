# Lab 03 — Diagnose a Hostname and SAN Mismatch

## Incident Summary

**Target System:** Staff scheduling portal (simulated via wrong.host.badssl.com)

**Reported Behavior:** Browser security errors after the portal was moved to a new hostname — staff cannot access the scheduling system.

**Diagnostic Scope:** PKI Diagnostic Framework — all 4 steps

## Diagnostic Steps

**Step 1 — Retrieve:**
Used OpenSSL s_client to connect to wrong.host.badssl.com:443 and retrieve the server certificate. Observed the certificate chain and TLS handshake output.

**Step 2 — Parse:**
Parsed the certificate using OpenSSL x509 to extract details such as Subject CN and Subject Alternative Names (SANs).

**Step 3 — Validate the Chain:**
Observed verify return code 20 (unable to get local issuer certificate), indicating a chain validation issue, but continued analysis of hostname validation.

**Step 4 — Check Revocation and Trust:**
Reviewed certificate trust and hostname validation. Determined that the certificate is trusted but does not match the requested hostname.

## Evidence

- Hostname accessed (what the client expected): wrong.host.badssl.com  
- Subject CN (what the certificate says): *.badssl.com  
- SAN entries (full list): *.badssl.com, badssl.com  
- Do any SAN entries match the hostname? (yes/no): No  
- Verify return code from openssl s_client: 20 (unable to get local issuer certificate)  
- Does the chain validate independently of the hostname issue? (yes/no): Yes  
- OCSP URL present? (yes/no): Yes  

## Root Cause

This is a certificate hostname mismatch problem, not a chain problem or configuration failure. The certificate is valid and trusted, but it does not cover the requested hostname. The wildcard certificate (*.badssl.com) only covers one subdomain level and does not match wrong.host.badssl.com, which has multiple subdomain levels.

## Remediation

Step-by-step path to resolve this incident:

1. Issue a new certificate that explicitly includes wrong.host.badssl.com in the SAN field.  
2. Alternatively, update DNS to use a hostname that matches the existing certificate (e.g., use a single-level subdomain).  
3. Deploy the corrected certificate on the server and restart the service.  

## Why a DNS CNAME alias would not fix this

A DNS CNAME alias would not resolve this issue because TLS validation occurs before DNS aliasing is considered complete. The client validates the hostname against the certificate presented by the server. Even if a CNAME points to a valid host, the certificate must still match the original hostname requested by the client.

## Key Findings

- The certificate is valid but does not match the requested hostname.  
- Wildcard certificates only cover one level of subdomains.  
- Hostname mismatches result in TLS failure even when the certificate is otherwise valid.  

## Challenges / Troubleshooting

- Initial confusion due to chain validation warning (verify return code 20).  
- Required understanding of wildcard certificate limitations.  

## Artifacts

- san_cert.pem
