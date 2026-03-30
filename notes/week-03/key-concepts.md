# Week 3 Key Concepts

## X.509 Certificate Structure
An X.509 certificate contains key information like  
subject, issuer, validity period, public key,  
and signature.

These fields define identity and trust.

---

## Subject Alternative Name (SAN)
SAN lists the valid domain names for a certificate.

Browsers rely on SAN instead of the Common Name  
to verify identity.

---

## Key Usage
Key Usage defines what actions a certificate  
is allowed to perform.

Examples include digital signature and key encipherment.

---

## Extended Key Usage (EKU)
EKU defines the specific purpose of a certificate.

For example, server authentication or client authentication.

---

## Certificate Chain and Trust
Certificates form a chain from the server  
to an intermediate to a root certificate.

Trust is established when the full chain is valid  
and linked to a trusted root.
