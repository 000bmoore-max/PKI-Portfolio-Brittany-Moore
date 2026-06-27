# Lab 03: Compare Certificate Profiles Side by Side

**Student Name:** Brittany Moore
**Date Completed:** June 27, 2026
**Phase:** 2 | **Week:** 11
**Submission Path:** `labs/week-11/lab-03-profile-comparison.md`

---

## Overview

Lab 03 compares the three certificate profiles issued during Weeks 10 and 11: a TLS certificate, a service account certificate, and a code signing certificate. The purpose of this exercise is to understand how Key Usage, Enhanced Key Usage (EKU), subject naming, and enrollment settings differ based on the intended cryptographic purpose of the certificate.

---

## Pre-Lab — Locate All Three Certificates

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready.

### Step 1 — List All Certificates in the Personal Store

```powershell
certutil -store My
```

**Week 10 TLS certificate present:**

- [x] Yes — Thumbprint: `241A6514FBC8904C2059212D826CE0D8D650FD70`

### Step 2 — Locate the Service Account Certificate

```powershell
certutil -store -service svc.autoenroll My
```

The service account certificate was located and identified through the previously issued service account enrollment.

### Step 3 — Record All Three Thumbprints

| Certificate | Template | Thumbprint |
|-------------|----------|------------|
| TLS (Week 10, Lab 02) | CVI-WebServer | 241A6514FBC8904C2059212D826CE0D8D650FD70 |
| Service Account (Lab 01) | CVI-ServiceAccount | 507CCD432A09838FBDCC26AD539F94F39D8F0850 |
| Code Signing (Lab 02) | CVI-CodeSigning | F6C93232958E217C74D4BE60AFA29351E3048593 |

---

# Part A — Inspect All Three Certificates

## Certificate 1 — TLS (CVI-WebServer)

```powershell
certutil -store My "241A6514FBC8904C2059212D826CE0D8D650FD70"
```

**Certificate information:**

- Serial Number: `44000000033c2d101c6fe68073000000000003`
- Issuer: `CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local`
- Subject: `CN=webserver.corp.cvilab.local`
- Template: `CVI-WebServer`
- Validity: `6/18/2026 8:39 PM – 4/25/2027 7:36 PM`

---

## Certificate 2 — Service Account (CVI-ServiceAccount)

```powershell
certutil -store -service svc.autoenroll My
```

**Certificate information:**

- Serial Number: `440000000658182b06d62dc4fb000000000006`
- Issuer: `CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local`
- Subject: `Principal Name=pki.admin@corp.cvilab.local`
- Template: `CVI-ServiceAccount`
- Validity: `6/26/2026 3:07 PM – 4/25/2027 7:36 PM`

---

## Certificate 3 — Code Signing (CVI-CodeSigning)

```powershell
certutil -store My "F6C93232958E217C74D4BE60AFA29351E3048593"
```

**Certificate information:**

- Serial Number: `4400000007022f1f329200224d000000000007`
- Issuer: `CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local`
- Subject: `CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local`
- Template: `CVI-CodeSigning`
- Validity: `6/26/2026 6:12 PM – 4/25/2027 7:36 PM`

---

## Comparison Table

| Field | TLS Certificate | Service Account Certificate | Code Signing Certificate |
|-------|----------------|----------------------------|--------------------------|
| Template Name | CVI-WebServer | CVI-ServiceAccount | CVI-CodeSigning |
| Subject | CN=webserver.corp.cvilab.local | Principal Name=pki.admin@corp.cvilab.local | CN=PKI Admin, OU=PKI Admins |
| Subject Source | Supplied in request | Build from AD | Build from AD |
| Issuer | CVI Issuing CA 1 | CVI Issuing CA 1 | CVI Issuing CA 1 |
| Key Usage | Digital Signature, Key Encipherment | Digital Signature, Key Encipherment | Digital Signature |
| EKU | Server Authentication | Client Authentication | Code Signing |
| EKU OID(s) | 1.3.6.1.5.5.7.3.1 | 1.3.6.1.5.5.7.3.2 | 1.3.6.1.5.5.7.3.3 |
| Validity Period | 1 Year | 1 Year | 1 Year |
| Serial Number | 44000000033c2d101c6fe68073000000000003 | 440000000658182b06d62dc4fb000000000006 | 4400000007022f1f329200224d000000000007 |
| Thumbprint | 241A6514FBC8904C2059212D826CE0D8D650FD70 | 507CCD432A09838FBDCC26AD539F94F39D8F0850 | F6C93232958E217C74D4BE60AFA29351E3048593 |
| Request ID | 3 | 6 | 7 |

---

# Part B — Written Analysis

## 1 — Key Usage

The TLS certificate requires both Digital Signature and Key Encipherment because TLS relies on cryptographic operations that authenticate the server and establish encrypted communication sessions. The web server must be able to prove its identity and participate in key exchange operations during the TLS handshake.

The service account certificate also requires Digital Signature and Key Encipherment because it is used for client authentication and secure authentication operations. The certificate must support proving identity while also participating in secure key establishment processes.

The code signing certificate only requires Digital Signature because its sole purpose is to digitally sign executable code and scripts. It does not encrypt communications or participate in key exchange. The Digital Signature key usage allows software publishers to create cryptographic signatures that verify the authenticity and integrity of software.

---

## 2 — Extended Key Usage (EKU)

The TLS certificate contains the Server Authentication EKU because it is intended to be presented by a web server during a TLS handshake. Browsers and operating systems verify this EKU before establishing a trusted HTTPS session. If the Server Authentication EKU were missing, browsers would reject or warn about the certificate.

The service account certificate contains the Client Authentication EKU because it is used to authenticate a user or service identity to another system. Applications and operating systems performing certificate-based authentication verify that this EKU is present before accepting the credential.

The code signing certificate contains the Code Signing EKU because it is used to digitally sign scripts and executable files. PowerShell, Windows Defender Application Control, SmartScreen, and other operating system components verify the presence of the Code Signing EKU before treating the signature as valid for software authenticity purposes.

---

## 3 — Subject Name Source

The TLS certificate uses "Supplied in request" because the certificate subject must match the DNS name of the server being protected. Active Directory does not necessarily contain the exact DNS names, SAN entries, or hostnames required by web servers and applications. Allowing the requester to specify the subject ensures that the certificate accurately represents the service endpoint.

The service account and code signing certificates use "Build from Active Directory" because they represent identities already maintained within Active Directory. Using AD-generated subject names reduces administrative errors and ensures that the certificate identity matches the authorized account.

---

## 4 — Security Question

Combining Server Authentication, Client Authentication, and Code Signing into a single certificate would create a significant security risk. If an attacker obtained the private key, they could impersonate a trusted server, authenticate as a trusted user or service account, and sign malicious software that would appear legitimate.

Separating EKUs limits the impact of a compromised private key. By issuing distinct certificates for distinct purposes, organizations reduce the blast radius of a compromise and enforce the principle of least privilege within the PKI environment.

---

# Reflection

## Which certificate would be most critical to revoke?

The code signing certificate would be the most critical certificate to revoke immediately if its private key were compromised. A compromised code signing certificate could be used to sign malicious software, scripts, or executables that would appear trustworthy to users and security systems. Because signed software may be distributed broadly and remain trusted for long periods, the potential impact of compromise is significantly greater than that of a single server or service account certificate.

## What would you add to this comparison?

If presenting this analysis to a security team, I would add certificate template permissions, enrollment workflows, revocation procedures, CRL distribution points, authority information access locations, and private key protection requirements. These operational controls provide additional insight into the security posture and risk management strategy of the PKI environment.

---

# Submission Checklist

- [x] Pre-lab: All three thumbprints recorded in the table
- [x] Pre-lab: Service account cert located
- [x] Part A: certutil output documented
- [x] Part A: Comparison table fully completed
- [x] Part B: Key Usage analysis completed
- [x] Part B: EKU analysis completed
- [x] Part B: Subject Name source analysis completed
- [x] Part B: Security question answered
- [x] Reflection completed
- [x] File saved as `lab-03-profile-comparison.md`
- [x] All three Week 11 lab files committed together
- [x] Request IDs recorded for Week 12
