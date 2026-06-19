# Lab 02: Issue Your First Certificate from a Custom Template

**Student Name:** Brittany Moore
**Date Completed:** 6/18/2026
**Phase:** 2 | **Week:** 10
**Submission Path:** `labs/week-10/lab-02-first-issuance.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Also confirm the following before starting:

```powershell
Get-Service -Name CertSvc
certutil -ping
```

**CertSvc status:** Running

**CA responding (certutil -ping):**
- [x] Yes
- [ ] No — action taken:

**CVI-WebServer template visible in certtmpl.msc (from Lab 01):**
- [x] Yes
- [ ] No — complete Lab 01 before proceeding

> **Lab 01 is required before Lab 02.** The CVI-WebServer template must exist in certtmpl.msc before you can publish it to the CA in Part A.

---

## Part A — Publish the Template to the CA

Publishing the template makes it available for certificate requests. The template definition lives in Active Directory — this step tells the CA to read it.

### Step 1 — Open the CA Management Console

### Step 2 — Publish the Template

**CVI-WebServer visible in Certificate Templates node:**
- [x] Yes
- [ ] No — describe what happened:

```text
N/A
```

**What you observe in the Certificate Templates node after publishing:**

```text
The CVI-WebServer template appeared in the Certificate Templates node after publishing. The CA successfully recognized the template and made it available for enrollment requests.
```

---

## Part B — Request the Certificate via MMC

### Step 1 — Open MMC and Add the Certificates Snap-in

### Step 2 — Navigate to the Certificate Store

### Step 3 — Launch the Certificate Enrollment Wizard

### Step 4 — Select Enrollment Policy

**Enrollment policy selected:**

```text
Active Directory Enrollment Policy
```

### Step 5 — Select the CVI-WebServer Template

**Templates visible in the wizard:**

```text
Computer
CVI-WebServer
```

**CVI-WebServer visible in the wizard:**
- [x] Yes
- [ ] No — troubleshooting steps taken:

**Subject name entered:**

```text
webserver.corp.cvilab.local
```

### Step 6 — Enroll

**Certificate request outcome:**
- [x] Issued immediately (Status: Succeeded)
- [ ] Pending manager approval — describe resolution:
- [ ] Error encountered:

```text
N/A
```

---

## Part C — Inspect the Issued Certificate

### Step 1 — Inspect in the MMC Certificates Snap-in

**General tab:**

| Field | Value |
|-------|-------|
| Issued to | webserver.corp.cvilab.local |
| Issued by | CVI Issuing CA 1 |
| Valid from | 6/18/2026 |
| Valid to | 4/25/2027 |

**Details tab — record the following:**

| Field | Value |
|-------|-------|
| Serial Number | 44000000033c2d101c6fe68073000000000003 |
| Signature Algorithm | sha256RSA |
| Subject | CN=webserver.corp.cvilab.local |
| Key Usage | Digital Signature, Key Encipherment |
| Enhanced Key Usage | Server Authentication |
| Subject Alternative Name (if present) | Not Present |
| Thumbprint | 241a6514fbc8904c2059212d826ce0d8d650fd70 |

### Step 2 — Inspect with certutil

**Full certutil output:**

```text
Certificate 1

Serial Number: 44000000033c2d101c6fe68073000000000003
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
NotBefore: 6/18/2026 8:39 PM
NotAfter: 4/25/2027 7:36 PM
Subject: CN=webserver.corp.cvilab.local
Template: CVI-WebServer
Cert Hash(sha1): 241a6514fbc8904c2059212d826ce0d8d650fd70
```

### Step 3 — Confirm in certsrv.msc Issued Certificates Node

**Certificate visible in Issued Certificates node:**
- [x] Yes

**Record from the Issued Certificates node:**

| Column | Value |
|--------|-------|
| Request ID | 3 |
| Requester Name | CORP\PKI-SRV01$ |
| Certificate Template | CVI-WebServer |
| Issued Common Name | webserver.corp.cvilab.local |
| Certificate Expiration Date | 4/25/2027 |

> **Save your Request ID.** You will use it in Week 12 to revoke this certificate, and it is also needed for the Week 11 Lab 03 profile comparison exercise.

---

## Part D — Write-Up: The Issuance Workflow

Describe the full certificate issuance workflow in your own words, in plain prose paragraphs (not bullet points). Cover all four stages:

1. What happened in Active Directory when you published the template.
2. What the MMC Certificate Enrollment wizard sent to the CA.
3. What the CA evaluated before issuing the certificate.
4. Where the issued certificate was placed and why.

Publishing the CVI-WebServer template made the template available to the Certification Authority. The template already existed in Active Directory, but publishing it instructed the CA to load the template and allow certificate requests based on its configuration. Once published, users and computers with the appropriate permissions could see and request certificates using that template.

During enrollment, the MMC Certificate Enrollment Wizard generated a Certificate Signing Request (CSR). The CSR contained information such as the subject name, public key, and template information. The CA evaluated the request by verifying enrollment permissions, checking template settings, and ensuring all required information was present. After validation, the CA issued the certificate and signed it using its private key.

The issued certificate was placed into the local certificate store where enrollment occurred. A record of the issuance was also created in the Certification Authority database under Issued Certificates. This record allows administrators to track, audit, renew, and revoke certificates when necessary.

**One thing about the issuance process you did not expect or want to understand better:**

```text
I was surprised that publishing a template in Active Directory does not automatically make it available on the Certification Authority. I learned that the template must also be published to the CA before certificate enrollment can occur.

```
## Part D — Write-Up: The Issuance Workflow

Publishing the CVI-WebServer template made the template available to the Certification Authority. The template already existed in Active Directory, but publishing it instructed the CA to load the template and allow certificate requests based on its configuration. Once published, users and computers with the appropriate permissions could see and request certificates using that template.

During enrollment, the MMC Certificate Enrollment Wizard generated a Certificate Signing Request (CSR). The CSR contained information such as the subject name, public key, and template information. I supplied the common name webserver.corp.cvilab.local because the template was configured to use “Supply in the request.” The Certification Authority evaluated the request by checking enrollment permissions, validating the template settings, and confirming that all required information was present. After those checks were completed, the CA issued the certificate and signed it with its private key.

The issued certificate was placed in the Local Computer certificate store on PKI-SRV01. A record of the certificate issuance was also created in the Certification Authority database under Issued Certificates. This record allows administrators to track, audit, renew, and revoke certificates throughout the certificate lifecycle.

**One thing about the issuance process I did not expect or want to understand better:**

I was surprised that creating a template in Active Directory was not enough to begin issuing certificates. I learned that the template must also be published to the Certification Authority before it becomes available for enrollment requests.


---

## Submission Checklist

- [x] Pre-lab verification completed — CertSvc running, CA responding, CVI-WebServer template confirmed in certtmpl.msc
- [x] Part A: CVI-WebServer template published to CVI Issuing CA 1 via certsrv.msc
- [x] Part A: Template visible in Certificate Templates node — confirmed
- [x] Part B: MMC opened with Certificates snap-in
- [x] Part B: Active Directory Enrollment Policy selected
- [x] Part B: CVI-WebServer template located in wizard
- [x] Part B: Subject name supplied (webserver.corp.cvilab.local)
- [x] Part B: Certificate enrollment outcome documented
- [x] Part C: Certificate details recorded from MMC
- [x] Part C: certutil output pasted
- [x] Part C: Certificate confirmed in certsrv.msc Issued Certificates node
- [x] Part C: Request ID recorded
- [x] Part D: Issuance workflow write-up completed
- [x] File saved as `lab-02-first-issuance.md`
- [x] File committed to portfolio repo under `labs/week-10/`
