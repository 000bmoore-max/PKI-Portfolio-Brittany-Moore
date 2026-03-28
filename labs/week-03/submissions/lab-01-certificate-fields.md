# Lab 01 — Inspect X.509 Certificate Fields

## Overview
This lab focused on inspecting an X.509 certificate to understand how digital certificates are used in PKI. The goal was to analyze certificate fields such as issuer, subject, validity period, and public key algorithm to understand how trust is established in secure communications.

---

## Environment
- OS: Windows
- Terminal used: OpenSSL Command Prompt
- OpenSSL version: OpenSSL 3.6.1

---

## Certificate Fields

| Version              | 3 |
| Serial Number        |b1:12:df:9c:52:57:5f:1a:12:8e:17:31:4f:9f:d3:b1|
| Signature Algorithm  | ecdsa-with-SHA256 |
| Issuer               | Google Trust Services |
| Subject              | www.google.com |
| Not Before           | Mar 09 2026 |
| Not After            | Jun 01 2026 |
| Public Key Algorithm | id-ecPublicKey |

---

## Observations

1. The certificate was issued by Google Trust Services.
2. The certificate represents the domain google.com.
3. The certificate expires on the "Not After" date listed in the certificate output.
4. The public key algorithm used is ecdsa-with-SHA256.
5. The Issuer field matters because it identifies the trusted Certificate Authority (CA) that validated and signed the certificate, which allows systems to verify authenticity.
