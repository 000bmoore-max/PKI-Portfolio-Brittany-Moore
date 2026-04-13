# Week 6 — Notes

## PKI Diagnostic Process
The troubleshooting process for TLS issues follows four steps:
1. Retrieve the certificate using OpenSSL
2. Parse the certificate to examine details like CN and SAN
3. Validate the certificate chain to ensure trust path exists
4. Check revocation and overall trust status

---

## Expired Certificate
- Certificates contain a validity period (NotBefore and NotAfter)
- If current date is past NotAfter, the certificate is rejected
- Even a valid chain cannot override expiration
- This results in immediate TLS failure

---

## Broken Certificate Chain
- The server must send the full certificate chain (leaf + intermediate)
- If the intermediate certificate is missing, clients cannot verify trust
- OpenSSL shows errors like "unable to get local issuer certificate"
- Fix requires adding the missing intermediate certificate

---

## SAN / Hostname Mismatch
- TLS validates that the hostname matches the certificate
- The SAN field defines valid domains for the certificate
- Wildcards like *.domain.com only match one subdomain level
- Multi-level domains (e.g., wrong.host.domain.com) will fail
- This causes browser security warnings

---

## Tools Used
- openssl s_client → retrieve certificates
- openssl x509 -text → inspect certificate details
- openssl verify → check trust chain

---

## Important Distinction
Different TLS failures come from different causes:
- Expired = time issue
- Broken chain = trust path issue
- SAN mismatch = identity issue

Understanding this distinction helps determine the correct fix.
- Expiration
- Trust chain issues
- Identity mismatch

Once you identify the category, the fix becomes straightforward.
