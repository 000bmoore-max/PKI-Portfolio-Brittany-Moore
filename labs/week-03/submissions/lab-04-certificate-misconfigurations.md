# Lab 04 — Detect Certificate Misconfigurations

## Overview
This lab was about identifying and analyzing common certificate  
misconfigurations that can break trust in a PKI environment.  

The goal was to understand how issues like missing SAN,  
incorrect EKU, expired certificates, and missing intermediate  
certificates affect browser trust and HTTPS connections.

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**  
No, modern browsers would not trust this certificate.

**Analysis:**  
Modern browsers require the Subject Alternative Name (SAN) field  
to validate domain names.  

The Common Name (CN) is no longer sufficient for hostname validation.  

If SAN is missing, the browser cannot verify the domain,  
resulting in an error such as `NET::ERR_CERT_COMMON_NAME_INVALID`.

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**  
No, the browser would not accept this certificate.

**Analysis:**  
Extended Key Usage (EKU) defines what the certificate is allowed  
to be used for.  

For HTTPS, the certificate must include  
"TLS Web Server Authentication."  

If this value is missing or incorrect,  
the browser will reject the certificate because it is not  
authorized for server authentication.

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**  
The browser will reject the certificate  
and display a security warning.

**Analysis:**  
Certificates have a defined validity period.  

If the current date is outside that range,  
the certificate is considered invalid.  

This causes browsers to block access or show warnings  
because the certificate can no longer be trusted.  

Proper certificate lifecycle management is important  
to prevent outages.

---

## Scenario 4 — Missing Intermediate Certificate

**Can the browser build a complete trust chain?**  
No, the browser cannot build a complete trust chain.

**Analysis:**  
A full certificate chain includes:
- the server certificate  
- intermediate certificate(s)  
- a trusted root certificate  

If the intermediate certificate is missing,  
the browser cannot link the server certificate  
to a trusted root authority.  

This results in trust errors even if the server certificate  
itself is valid.

---

## Key Takeaway
The most important thing I learned is that even small  
certificate misconfigurations can completely break trust  
in a system.  

Proper configuration of SAN, EKU, certificate validity,  
and full chain delivery is critical to ensuring  
secure and trusted HTTPS connections.
