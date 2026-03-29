# Lab 03 — Verify a Certificate Chain

## Overview
This lab focused on verifying a certificate chain using OpenSSL. The goal was to understand how trust is established between a server certificate, intermediate certificate, and root certificate in a PKI system.

## Environment
- OS: Windows
- Tool: OpenSSL

## Chain Verification Result
The certificate chain was verified using the OpenSSL verify command.

Output:
server.pem: OK

## Certificate Roles

| Certificate        | Role          | Key Indicator |
|--------------------|--------------|--------------|
| root.pem           | Root CA       | Self-signed, top of trust chain |
| intermediate.pem   | Intermediate CA | Issued by root, signs server certs |
| server.pem         | Leaf/Server Cert | Issued to *.google.com |

## Observations

1. Yes, the chain verified successfully. The output showed "server.pem: OK".
2. The root CA was identified as the top-level certificate that is self-signed.
3. The intermediate CA was identified as the certificate between the root and server.
4. The Basic Constraints field confirms whether a certificate can issue other certificates.
5. Removing the intermediate certificate breaks the chain because the server certificate can no longer be linked to a trusted root.

## Challenges / Troubleshooting
I initially had issues with file naming and file extensions (.pem), and locating saved files. After correcting file names and ensuring all certificates were in the same directory, the verification command worked successfully.
