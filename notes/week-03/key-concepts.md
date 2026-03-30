# Week 3 Key Concepts

## Digital Certificates
Digital certificates are used to verify the identity  
of a system or website.

They contain information like the subject, issuer,  
and public key.

Certificates are a key part of PKI and enable secure  
communication over networks.

---

## Certificate Extensions
Certificate extensions define what a certificate  
is allowed to do.

Important extensions include Subject Alternative Name (SAN),  
Key Usage, and Extended Key Usage (EKU).

These fields control how the certificate can be used  
in real-world scenarios.

---

## Subject Alternative Name (SAN)
SAN lists the valid domain names associated  
with a certificate.

Modern browsers rely on SAN instead of the Common Name  
to verify a website’s identity.

If SAN is missing or incorrect, the certificate  
will not be trusted.

---

## Certificate Chain
A certificate chain connects a server certificate  
to a trusted root certificate.

It includes the server certificate, intermediate  
certificate(s), and root certificate.

Each certificate in the chain must be valid  
for trust to be established.

---

## Certificate Misconfigurations
Misconfigurations like missing SAN, incorrect EKU,  
expired certificates, or missing intermediates  
can break trust.

These issues cause browsers to display security warnings  
and block access to websites.

Understanding these helps identify and fix  
real-world security problems.
