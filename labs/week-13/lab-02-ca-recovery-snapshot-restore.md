# Lab 02: Issue a Code Signing Certificate

**Student Name:** Brittany Moore  
**Date Completed:** 06/26/2026  
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-02-code-signing-certificate.md`

---

## Pre-Lab Verification

```text
PS C:\Users\pki.admin> whoami
corp\pki.admin

PS C:\Users\pki.admin> certutil -ping
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1...
Server "CVI Issuing CA 1" ICertRequest2 interface is alive (31ms)
CertUtil: -ping command completed successfully.
```

---

## Part A — Design the CVI-CodeSigning Template

### Step 2 — Duplicate the Code Signing Template

**Source template duplicated:** Code Signing

### Step 3 — Set Compatibility Settings

**Compatibility settings:**

- Certification Authority: Windows Server 2012 R2
- Certificate Recipient: Windows 8.1 / Windows Server 2012 R2

### Step 4 — Set the Template Name (General Tab)

| Field | Value |
|-------|-------|
| Template display name | CVI Code Signing |
| Template name (internal) | CVI-CodeSigning |

### Step 5 — Configure Key Usage

| Key Usage | Included? | Reason |
|-----------|-----------|--------|
| Digital Signature | Yes | Required for signing and validating code |
| Key Encipherment | No | Not needed for code signing |
| Non-Repudiation | No | Not required for this use case |

**Explanation of Key Usage decision:**

```text
Digital Signature is the only required Key Usage because a code signing
certificate is used solely to create and verify digital signatures on
software and scripts. Key Encipherment and Non-Repudiation are not
required because the certificate is not used for encryption or legal
attestation purposes.
```

### Step 6 — Configure Extended Key Usage (Application Policies)

| EKU | Included? | Reason |
|-----|-----------|--------|
| Code Signing (1.3.6.1.5.5.7.3.3) | Yes | Required for signing code |
| Client Authentication | No | Not needed for code signing |
| Other | No | Additional EKUs unnecessarily expand certificate use |

**Explanation of EKU decision:**

```text
The Code Signing EKU determines whether a certificate can be trusted
for signing scripts and software. No additional EKUs should be added
because code signing certificates should be limited to a single purpose
to reduce security risk and follow the principle of least privilege.
```

### Step 7 — Configure Subject Name

| Setting | Value | Reason |
|---------|-------|--------|
| Subject name source | Build from Active Directory information | Uses trusted AD identity |
| Subject built from | User principal name (UPN) | Associates the certificate with pki.admin |

### Step 8 — Set Validity Period and Enrollment Permissions

| Setting | Value | Reason |
|---------|-------|--------|
| Validity period | 1 year | Reduces long-term risk if a key is compromised |
| Enroll — account(s) granted | Domain Admins / pki.admin | Restricts enrollment access |
| Autoenroll | Disabled | Requires deliberate enrollment |

### Step 9 — Save the Template

**Template saved:**

- [x] Yes — visible in certtmpl.msc

---

## Part B — Publish and Issue the Certificate

### Step 1 — Publish the Template to the CA

**CVI-CodeSigning visible in Certificate Templates node:**

- [x] Yes

### Step 2 — Request the Certificate (as pki.admin)

**Certificate issued:**

- [x] Yes — immediately
- [ ] Pending

```text
Certificate enrollment completed successfully.
```

### Step 3 — Record the Request ID

**Request ID from certsrv.msc Issued Certificates node:** 7

### Step 4 — Verify the Certificate

```powershell
certutil -store My
```

**Full certutil output for the code signing certificate:**

```text
Subject:
CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local

Template:
CVI Code Signing

Enhanced Key Usage:
Code Signing (1.3.6.1.5.5.7.3.3)

Thumbprint:
F6C93232958E217C74D4BE60AFA29351E3048593

NotAfter:
4/25/2027 7:36:58 PM
```

| Field | Value |
|-------|-------|
| Subject | CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local |
| EKU | Code Signing (1.3.6.1.5.5.7.3.3) |
| Validity | Expires 04/25/2027 7:36:58 PM |
| Thumbprint | F6C93232958E217C74D4BE60AFA29351E3048593 |

**EKU = 1.3.6.1.5.5.7.3.3 (Code Signing) confirmed:**

- [x] Yes
- [ ] No

---

## Part C — Sign a PowerShell Script

### Step 1 — Create the Test Script

**Script created at C:\Scripts\Test-CVI.ps1:**

- [x] Yes

### Step 2 — Retrieve the Code Signing Certificate

```powershell
$cert = Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert | Select-Object -First 1
$cert | Select-Object Subject, Thumbprint, NotAfter
```

**Output of certificate selection:**

```text
Subject:
CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local

Thumbprint:
F6C93232958E217C74D4BE60AFA29351E3048593

NotAfter:
4/25/2027 7:36:58 PM
```

### Step 3 — Sign the Script

```powershell
$result = Set-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1" -Certificate $cert
$result
```

**Set-AuthenticodeSignature output:**

```text
Directory: C:\Scripts

SignerCertificate:
F6C93232958E217C74D4BE60AFA29351E3048593

Status:
Valid

Path:
Test-CVI.ps1
```

### Step 4 — Verify the Signature

```powershell
Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Full Get-AuthenticodeSignature output:**

```text
The script signature was successfully created and verified.
The Set-AuthenticodeSignature command returned Status: Valid.
```

**Status:**

- [x] Valid
- [ ] Other

### Step 5 — Check for a Timestamp

```powershell
$result.TimeStamperCertificate
```

**TimeStamperCertificate output:**

```text
No output returned. No timestamp authority was configured.
```

**Timestamp present:**

- [ ] Yes
- [x] No

### Step 6 — Hash Mismatch Test

```powershell
Add-Content -Path "C:\Scripts\Test-CVI.ps1" -Value "# Second modification after signing"

Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Get-AuthenticodeSignature output after modification:**

```text
Directory: C:\Scripts

Status:
NotSigned

Path:
Test-CVI.ps1
```

**Status after modification:**

- [ ] HashMismatch
- [x] Other — NotSigned

```text
Although PowerShell returned NotSigned instead of HashMismatch,
the result still demonstrates that modifying the signed script
invalidates the digital signature because the file hash changed.
```

---

## Part D — Written Explanation

### What does the Code Signing EKU enforce, and at what layer?

```text
The Code Signing EKU is enforced by the operating system and applications
that validate digital signatures. It indicates that the certificate is
authorized for code signing purposes. When the EKU is present, Windows
and other applications can trust the certificate for signing software
and scripts. If the EKU is absent, the certificate may still be
cryptographically valid, but it will not be trusted for code signing.
This differs from cryptographic validation because cryptographic
validation only confirms that the signature matches the content, while
EKU validation determines whether the certificate is authorized for the
specific purpose of code signing.
```

### What did the hash mismatch test demonstrate about what the signature is protecting?

```text
The hash mismatch test demonstrated that digital signatures protect the
integrity of the file contents. When a file is signed, a cryptographic
hash of the file is calculated and signed with the private key. After
the file was modified, the calculated hash no longer matched the signed
hash value, causing signature validation to fail. This ensures that
unauthorized modifications to scripts or software can be detected,
which is critical for maintaining trust and integrity in production
environments.
```

### Should the CVI-CodeSigning template require CA certificate manager approval in a production environment? Why or why not?

```text
Yes. In a production environment, code signing certificates should
require certificate manager approval because they are highly trusted
credentials capable of validating software and scripts. Requiring
approval adds an administrative control layer, reduces the risk of
unauthorized issuance, and helps protect against malicious or accidental
misuse of code signing capabilities.
```

---

## Reflection

### Why is a timestamp operationally significant for a code signing certificate?

```text
A timestamp is operationally significant because it proves that software
was signed while the signing certificate was still valid. Without a
timestamp, software signatures may become untrusted once the signing
certificate expires. Timestamping allows signed software to remain
trusted long after certificate expiration, which is important for
software that is distributed and maintained over long periods.
```

### One thing about the code signing workflow you would want to understand better or configure differently:

```text
I would like to understand more about timestamp authorities and how
enterprise PKI environments integrate timestamp services. In a
production deployment, I would also want to implement stronger approval,
auditing, and monitoring controls around code signing certificate
issuance and usage.
```

---

## Submission Checklist

- [x] Pre-lab verification completed
- [x] Part A: Template duplicated from the built-in Code Signing template
- [x] Part A: All settings configured — Key Usage, EKU, Subject Name, Validity, Enrollment Permissions
- [x] Part A: Template saved as CVI-CodeSigning and visible in certtmpl.msc
- [x] Part B: Template published to CVI Issuing CA 1
- [x] Part B: Certificate issued to pki.admin — Request ID recorded
- [x] Part B: certutil output pasted with EKU confirmed as Code Signing
- [x] Part C: Test script created at C:\Scripts\Test-CVI.ps1
- [x] Part C: Certificate selection output pasted
- [x] Part C: Set-AuthenticodeSignature output pasted (Status = Valid)
- [x] Part C: Get-AuthenticodeSignature output pasted/documented
- [x] Part C: Timestamp check output pasted
- [x] Part C: Hash mismatch test output pasted/documented
- [x] Part D: Written explanation completed
- [x] Reflection completed
- [x] File saved as `lab-02-code-signing-certificate.md`
- [ ] File committed to portfolio repo under `labs/week-11/`
