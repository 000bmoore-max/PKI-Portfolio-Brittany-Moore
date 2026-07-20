# Lab 01 — AD CS Event Log Navigation

**Student Name:** Brittany Moore

**Date Completed:** July 20, 2026

**Phase:** 2 | **Week:** 14

**Submission Path:** `labs/week-14/lab-01-event-log-navigation.md`

---

# Pre-Lab Check

### whoami output

```text
corp\pki.admin
```

### Get-Service CertSvc

```text
Status : Running

Name : CertSvc

DisplayName : Active Directory Certificate Services
```

### certutil -ping

```text
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1 ...

Server "CVI Issuing CA 1"
ICertRequest2 interface is alive.

CertUtil: -ping command completed successfully.
```

---

# Part A — Application Event Log Navigation

**Application log filtered by Source = CertificationAuthority:** ☑ Yes

---

## Event 1

**Event ID:** 26

**Timestamp:** July 20, 2026 – 11:09:33 AM

**Description:**

```text
Active Directory Certificate Services for CVI Issuing CA 1 was started.
```

**What it represents:**

The Certificate Authority service successfully started and is available to issue certificates, publish CRLs, and process certificate requests.

---

## Event 2

**Event ID:** 38

**Timestamp:** July 17, 2026 – 3:54:46 PM

**Description:**

```text
Active Directory Certificate Services for CVI Issuing CA 1 was stopped.
```

**What it represents:**

The Certificate Authority service shut down normally, typically because of maintenance, configuration changes, or an administrative restart.

---

## Event 3

**Event ID:** 128

**Timestamp:** July 17, 2026 – 2:45:05 PM

**Description:**

```text
An Authority Key Identifier was passed as part of certificate request 12.
This feature has not been enabled.
```

**What it represents:**

The CA generated a warning indicating that an Authority Key Identifier was included in a certificate request, but the optional feature was not enabled. Certificate processing continued while recording the warning in the Application log.

---

## Additional Events Observed

- Event ID 44 (Error)
- Event ID 77 (Warning)

---

## Event Type Summary

| Event Type | Example Event ID | Meaning |
|------------|-----------------:|---------|
| CA Service Started | 26 | Certification Authority successfully started |
| CA Service Stopped | 38 | Certification Authority stopped normally |
| Configuration Warning | 128 | Warning regarding Authority Key Identifier processing |
| Errors | 44 | Operational error requiring administrator review |
| Warnings | 77 | CA warning requiring investigation |

---

# Part B — CA Certificate Database Query

## Issued Certificates

Command:

```powershell
certutil -view -restrict "Disposition=20"
```

**Result:**

- Total issued certificates found: **8**

---

## Revoked Certificates

Command:

```powershell
certutil -view -restrict "Disposition=21"
```

**Result:**

- Total revoked certificates found: **1**

---

## Requester Filter

Command:

```powershell
certutil -view -restrict "RequesterName=CORP\pki.admin"
```

**Result:**

- Requester: **CORP\pki.admin**
- Certificates returned: **2**

---

## Certificate Record 1

| Field | Value |
|-------|-------|
| Request ID | 12 |
| Requester | CORP\pki.admin |
| Template | OCSP Response Signing |
| Common Name | OCSP Response Signing Certificate |
| Disposition | Issued |
| Source Lab | Week 12 — OCSP Configuration |

---

## Certificate Record 2

| Field | Value |
|-------|-------|
| Request ID | 8 |
| Requester | CORP\pki.admin |
| Template | CVI Code Signing |
| Common Name | Code Signing Certificate |
| Disposition | Issued |
| Source Lab | Week 11 — Code Signing |

---

## Template Distribution

Command:

```powershell
certutil -view -out "CertificateTemplate"
```

Templates observed:

- CVI Code Signing
- OCSP Response Signing

---

# Part C — Analysis

## 1. One event log entry — what it tells you about CA operational state

Event ID 26 indicates that Active Directory Certificate Services successfully started. This confirms the Certification Authority is operational and ready to process certificate requests, publish certificate revocation lists, and perform normal PKI operations. A successful startup event is an important indicator that the CA is functioning correctly after maintenance or a system restart.

---

## 2. One certificate record — lifecycle, template, requester, status, and next steps

The OCSP Response Signing certificate was requested by **CORP\pki.admin** during the Week 12 OCSP lab. The certificate was successfully issued using the OCSP Response Signing template and is used to digitally sign OCSP responses. As the certificate approaches expiration, it should be renewed before its validity period ends to ensure uninterrupted revocation checking for clients.

---

## 3. How the event log and certificate database complement each other

The event log records operational activity performed by the Certification Authority, including service starts, shutdowns, warnings, and errors. The CA database stores certificate lifecycle information such as issued, pending, failed, and revoked certificates. For example, if a certificate request fails, the event log explains why the failure occurred while the CA database identifies the affected certificate request and its disposition. Together, these tools provide administrators with both operational and certificate-specific information needed to troubleshoot PKI issues.

---

# Lab Report Questions

## 1. Most frequent event type and what it tells you about the CA's most common operation

The most common events observed were informational service events such as Event ID 26. These indicate that the Certification Authority regularly starts and performs normal operational tasks, reflecting a healthy PKI environment.

---

## 2. Difference between the event log and the CA database

The event log records system activity, warnings, and operational events, while the CA database stores certificate records and request history. Without the event log, administrators would lose important troubleshooting information. Without the CA database, they would lose certificate lifecycle records including issued and revoked certificates.

---

## 3. Disposition 21 (Revoked)

A revoked certificate should be reviewed for its serial number, revocation date, revocation reason, requester, and certificate template. These details help administrators determine why the certificate was revoked and verify that clients receive updated revocation information through CRLs or OCSP.

---

## 4. What is present in the Application log by default, and what is absent that matters in production

The Application log records service starts, service stops, warnings, informational events, and operational errors. However, it does not provide advanced auditing such as detailed certificate enrollment activity, security auditing, or centralized monitoring. Production environments typically supplement these logs with SIEM platforms and additional auditing to improve visibility and incident response.

---

# Submission Checklist

- ☑ Logged in as CORP\pki.admin — whoami output included

- ☑ CA running and responding — Get-Service and certutil -ping output included

- ☑ Application event log filtered by Source = CertificationAuthority

- ☑ At least three distinct event types documented with Event ID, timestamp, and description

- ☑ Event type summary table completed

- ☑ certutil -view output included for issued, revoked, and requester-filtered queries

- ☑ At least two specific certificate records documented

- ☑ Template distribution documented

- ☑ All three Part C analysis questions answered

- ☑ All four lab report questions answered

---

