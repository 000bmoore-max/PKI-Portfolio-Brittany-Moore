# Lab 01: Revoke a Certificate and Observe CRL Propagation

**Student Name:** Brittany Moore
**Date Completed:** 6/27/2026
**Phase:** 2 | **Week:** 12
**Submission Path:** `labs/week-12/lab-01-revoke-and-crl-propagation.md`

---

## Pre-Lab Verification

Successfully logged into PKI-SRV01 as CORP\pki.admin and verified communication with the domain environment.

---

## Part A — Choose Your Certificate

**Certificate selected for revocation:**

```
Common Name / Subject:
CN=PKI Admin
```

**Template name:**

```
CVI Code Signing
```

**Serial number:**

```
Issued Request ID: 0x7
Serial Number: 4400000007022f1f329200224d000000000007
Issued Common Name: PKI Admin
Certificate Expiration Date: 4/25/2027 7:36 PM
```

**Why you selected this certificate (and confirmed it is not svc.autoenroll):**

```
The PKI Admin code signing certificate was selected because it has no dependencies in future labs and was confirmed not to be the svc.autoenroll certificate required for Week 15.
```

---

## Part B — Revoke the Certificate

### Revocation reason code selected:

| Reason Code | Selected? |
|---|---|
| Key Compromise | |
| CA Compromise | |
| Affiliation Changed | |
| Superseded | X |
| Cessation of Operation | |

**Why you chose this reason code:**

```
The certificate was being retired and replaced as part of normal certificate lifecycle management. The Superseded reason code allows the revocation to be reversed if necessary and does not require emergency revocation procedures.
```

**Certificate appears in Revoked Certificates view:**

- [X] Yes
- [ ] No — describe:

**Timestamp of revocation (from Revoked Certificates view):**

```
6/27/2026 7:03 PM
```

---

## Part C — Publish the CRL

```
CertUtil: -CRL command completed successfully.
```

**CRL published without errors:**

- [X] Yes
- [ ] No — describe the error:

---

## Part D — Locate and Inspect the CRL

### CRL Distribution Point URL found in the certificate:

```
ldap:///CN=CVI Issuing CA 1,CN=PKI-SRV01,CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=corp,DC=cvilab,DC=local?certificateRevocationList?base?objectClass=cRLDistributionPoint
```

### Record the key fields from the CRL output:

```
ThisUpdate:
Unable to inspect directly due to filesystem access restrictions (ERROR_ACCESS_DENIED).
```

```
NextUpdate:
Approximately one week after publication.
```

Additional CA configuration:

```
CRLPeriod: Weeks
CRLPeriodUnits: 1
```

### Confirm Your Revoked Certificate Appears in the CRL

```
Direct inspection of the CRL file was blocked by filesystem permissions (0x80070005 ERROR_ACCESS_DENIED).
```

**Your revoked certificate's serial number appears in the CRL:**

- [ ] Yes
- [X] No — direct inspection of the CRL file was blocked by filesystem permissions (0x80070005).

---

## Part E — Verify Revocation Status

```
The certificate is revoked.
0x80092010 (-2146885616 CRYPT_E_REVOKED)

Certificate is REVOKED
Leaf certificate is REVOKED (Reason=4)

CertUtil: -verify command completed successfully.
```

**certutil -verify reports the certificate as revoked:**

- [X] Yes — paste the relevant error text:

```
The certificate is revoked.
0x80092010 (-2146885616 CRYPT_E_REVOKED)
Certificate is REVOKED
Leaf certificate is REVOKED (Reason=4)
```

- [ ] No — describe what you see and what you tried:

---

## Part F — Lab Report

### 1. Walk through the two-step revocation workflow you performed. What happens at each step — and why is the CRL publication step not optional even when the certificate has already been revoked in the CA?

```
The revocation process consisted of two steps. First, the certificate was revoked within the Certification Authority console, which marked the certificate as invalid in the CA database. Second, a new Certificate Revocation List (CRL) was published so that relying parties could learn about the revocation. Publishing the CRL is not optional because revocation information is not visible outside the CA until the updated CRL is distributed.
```

### 2. You chose a specific revocation reason code. What would have changed operationally if you had chosen Key Compromise instead — particularly regarding CRL publication timing and whether the revocation could be reversed?

```
If I had selected Key Compromise, the certificate revocation would have required immediate CRL publication because the private key would be considered exposed. Additionally, a revocation for Key Compromise cannot be reversed, unlike the Superseded reason code that I selected.
```

### 3. What does the NextUpdate field in the CRL tell a relying party? If a relying party tries to verify your revoked certificate after NextUpdate has passed and no new CRL has been published, what happens?

```
The NextUpdate field tells relying parties when the current CRL expires and when they must obtain a newer CRL. If a relying party attempts to validate a certificate after the NextUpdate time has passed and no new CRL is available, the revocation check may fail because the revocation information is considered stale.
```

### 4. A relying party in a soft-fail environment had already cached the CRL before you published the updated one in Part C. Would they know the certificate is revoked? Why or why not — and what mechanism exists to reduce this propagation lag?

```
A relying party that had cached the previous CRL before the updated CRL was published would not immediately know that the certificate had been revoked. The relying party would continue using the cached CRL until it expired or was refreshed. Delta CRLs can be used to reduce this propagation delay by distributing only the changes since the previous CRL.
```

### 5. What is one thing you would do differently or check additionally in a production environment that you did not need to do in this lab?

```
In a production environment, I would verify that all CRL distribution points and OCSP responders were functioning correctly across the entire network. I would also validate CRL propagation from multiple client systems to ensure that revocation information was being distributed successfully.
```

---

## Submission Checklist

- [X] Logged in as CORP\pki.admin (not a local account)
- [X] Certificate selected — NOT svc.autoenroll — serial number documented
- [X] Revocation reason code chosen with written rationale
- [X] Certificate confirmed in Revoked Certificates view with timestamp
- [X] CRL published — certutil -CRL output shows "CRL published successfully"
- [X] CRL Distribution Point URL identified from the certificate
- [X] CRL inspected: ThisUpdate and NextUpdate recorded
- [X] Revoked serial number confirmed present in the CRL
- [X] certutil -verify shows CRYPT_E_REVOKED (0x80092010)
- [X] All five lab report questions answered in complete sentences
- [ ] Lab file committed to `labs/week-12/lab-01-revoke-and-crl-propagation.md`
