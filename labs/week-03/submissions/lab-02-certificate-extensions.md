# Lab 02 — Investigate Certificate Extensions

## Overview
This lab focused on analyzing certificate extensions to understand how certificates define their purpose and usage in a PKI system. The goal was to identify key extensions and explain how they impact trust and functionality.

---

## Environment
- OS: Windows
- Tool: OpenSSL

---

## Extensions Found

### Subject Alternative Name (SAN)
The SAN field includes multiple domain names such as google.com, www.google.com, and other subdomains. This allows a single certificate to be used across multiple domains.

---

### Key Usage
The certificate allows Digital Signature and Key Encipherment, meaning it can be used to establish secure connections and encrypt data.

---

### Extended Key Usage (EKU)
The certificate is used for TLS Web Server Authentication, meaning it is valid for HTTPS websites.

---

### Basic Constraints
The certificate has CA: FALSE, meaning it is not a Certificate Authority and cannot issue other certificates.

---

## Observations

1. The SAN field contains multiple domains, allowing one certificate to secure several domain names.
2. The certificate is intended for secure web communication based on its key usage and extended key usage.
3. The certificate is not a CA, meaning it is used only for end-entity purposes like websites.
4. Extensions define how a certificate can be used and are critical for enforcing trust.

---

## Explanation

Certificate extensions determine how a certificate is used and trusted within a system. The SAN field ensures the certificate matches the domain being accessed, while Key Usage and EKU define what operations the certificate is allowed to perform. Basic Constraints ensures that only authorized certificates can act as Certificate Authorities. These extensions are essential for secure communication over HTTPS.

---

## Challenges / Troubleshooting
Initially encountered an error where the openssl command was not recognized in Powershell.
Resolved the issue by switching to the Win32 OpenSSL Command Promt, where OpenSSL was properly configured.
Faced confusion locating generated files (e.g., leaf_cert.pem) after running commmands.
Confirmed files were created in the current working directory and verified paths before continuing.
