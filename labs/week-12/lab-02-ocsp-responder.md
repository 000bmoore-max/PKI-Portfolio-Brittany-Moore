# Lab 02 — Configure and Test the OCSP Responder

**Student Name:** Brittany Moore  
**Date Completed:** July 17, 2026  
**Phase:** 2 | **Week:** 12  
**Submission Path:** `labs/week-12/lab-02-ocsp-responder.md`

---

## Objective

Configure and verify the Microsoft Online Responder role on PKI-SRV01, confirm that the responder has access to current revocation information, inspect the OCSP response-signing certificate, and test certificate revocation status using Windows certificate utilities.

---

## Part A — Online Responder Service Verification

### Command

```powershell
Get-Service OCSPSvc
```

### Result

```text
Status   Name      DisplayName
------   ----      -----------
Running  OCSPSvc   Online Responder Service
```

### Observation

The Microsoft Online Responder service was installed and running successfully on PKI-SRV01.

---

## Part B — Revocation Configuration

### Online Responder Configuration

| Setting | Observation |
|---|---|
| Server | `PKI-SRV01.corp.cvilab.local` |
| Revocation configuration name | `CVI Issuing CA 1 Revocation` |
| Certification authority | `CVI Issuing CA 1` |
| Responder service | Online Responder Service |
| Final status | Green — Working |
| Revocation provider | Operational |
| CRL source | Active Directory LDAP CDP |
| Delta CRL use | Disabled during troubleshooting |
| Signing certificate enrollment | Automatic enrollment enabled |

### Status Indicator

- [x] Green — Operational
- [ ] Yellow — Warning
- [ ] Red — Error

### Final Observation

The Online Responder revocation configuration for `CVI Issuing CA 1` displayed a green status and the word **Working**. This confirmed that the responder could access current CRL information and that the revocation provider was operational.

### Troubleshooting Performed

The revocation configuration initially displayed a red status with the following message:

```text
Revocation provider is not working on the Array controller.
```

The Application event log showed an Online Responder Revocation Provider error indicating that the responder had no usable CRL information or that the available CRL information was stale.

The base CRL was current, but the delta CRL was expired. Delta CRL publication was disabled with:

```powershell
certutil -setreg CA\CRLDeltaPeriodUnits 0
```

The Certificate Services service was restarted, and a new base CRL was published.

The current CRL was then published to the correct Active Directory CDP object with:

```powershell
certutil -dspublish -f "C:\Windows\System32\CertSrv\CertEnroll\CVI Issuing CA 1.crl" "PKI-SRV01" "CVI Issuing CA 1"
```

The Online Responder service was restarted:

```powershell
Restart-Service OCSPSvc
```

After refreshing the Online Responder console, the configuration changed to green and displayed **Working**.

---

## Part C — OCSP Response-Signing Certificate Verification

### Command

```powershell
certutil -store My
```

### Renewed OCSP Signing Certificate

| Certificate Property | Result |
|---|---|
| Subject | `CN=PKI-SRV01.corp.cvilab.local` |
| Issuer | `CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local` |
| Certificate template | `OCSPResponseSigning` |
| Template display name | OCSP Response Signing |
| Enhanced Key Usage | OCSP Signing |
| OCSP Signing EKU OID | `1.3.6.1.5.5.7.3.9` |
| OCSP NoCheck OID | `1.3.6.1.5.5.7.48.1.5` |
| Key usage | Digital Signature |
| Cryptographic provider | Microsoft Software Key Storage Provider |
| Private key exportable | No |
| Validity | Valid; renewed certificate expires in 2027 |
| Signature test | Passed |

### Certificate Renewal

The original OCSP signing certificate had expired on July 12, 2026. A renewed certificate was enrolled using:

```powershell
certreq -machine -enroll OCSPResponseSigning
```

The Online Responder service was then restarted:

```powershell
Restart-Service OCSPSvc
```

A new `OCSPResponseSigning` certificate was present with a future expiration date in 2027.

### Observation

The OCSP Signing EKU authorizes this certificate to digitally sign OCSP responses. The OCSP NoCheck extension prevents clients from attempting to perform a separate revocation check on the responder’s signing certificate.

---

## Part D — Certificate Revocation Testing

### Valid Certificate

The valid web-server certificate was exported from the local computer certificate store.

```powershell
Get-ChildItem Cert:\LocalMachine\My |
Where-Object { $_.Subject -eq "CN=webserver.corp.cvilab.local" } |
Export-Certificate -FilePath C:\valid.cer
```

### Valid Certificate Information

| Property | Result |
|---|---|
| File | `C:\valid.cer` |
| Subject | `webserver.corp.cvilab.local` |
| Template | `CVI-WebServer` |
| Issuer | `CVI Issuing CA 1` |
| AIA retrieval status | Verified |
| Certificate status | Valid / not revoked |

### URL Retrieval Test

```powershell
certutil -URL "C:\valid.cer"
```

The URL Retrieval Tool successfully retrieved and verified the issuing CA certificate from the certificate’s AIA location.

```text
Status: Verified
Type: Certificate (0)
Certificate Subject: webserver.corp.cvilab.local
```

### Revoked Certificate

The revoked certificate was saved as:

```text
C:\revoked.cer
```

### Revoked Certificate Information

| Property | Result |
|---|---|
| File | `C:\revoked.cer` |
| Subject | `CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local` |
| Issuer | `CVI Issuing CA 1` |
| Template | `CVI Code Signing` |
| Serial number | `4400000007022f1f329200224d000000000007` |
| Revocation result | Revoked |
| Error | `CRYPT_E_REVOKED` |
| Revocation reason | Reason 4 |

### Verification Command

```powershell
certutil -verify -urlfetch "C:\revoked.cer"
```

### Verification Output

```text
Subject:
    CN=PKI Admin
    OU=PKI Admins
    DC=corp
    DC=cvilab
    DC=local

Cert Serial Number:
    4400000007022f1f329200224d000000000007

The certificate is revoked.
0x80092010 (-2146885616 CRYPT_E_REVOKED)

Certificate is REVOKED
Leaf certificate is REVOKED (Reason=4)

CertUtil: -verify command completed successfully.
```

### Observation

The verification test correctly identified the PKI Admin certificate as revoked. This confirmed that the current revocation information was available to the Windows certificate validation process.

---

## Part E — Authority Information Access Inspection

### Command

```powershell
certutil -dump "C:\valid.cer" | findstr /i "OCSP Authority"
```

The filtered output did not display an OCSP URL, so the complete certificate was inspected with:

```powershell
certutil -dump "C:\valid.cer"
```

### AIA Result

```text
Authority Information Access

Access Method:
Certification Authority Issuer (1.3.6.1.5.5.7.48.2)

URL:
LDAP location for CVI Issuing CA 1
```

### Observation

The valid web-server certificate contained a Certification Authority Issuer AIA entry using LDAP. It did not contain a separate OCSP access method or OCSP responder URL.

This explains why the URL Retrieval Tool displayed a verified `Certificate (0)` result rather than a separate OCSP Good or Revoked result. The certificate could retrieve the issuing CA certificate through AIA, but it did not contain an OCSP URL that directly identified the Online Responder.

---

## Part F — Analysis Questions

### 1. What is the difference between a CRL and OCSP? Include the advantages and disadvantages of each method.

A Certificate Revocation List is a periodically published list containing the serial numbers of certificates that have been revoked by a certification authority. A client downloads the CRL and checks whether the certificate’s serial number appears on the list. CRLs can be cached and used when a client cannot contact the CA, but they can consume more bandwidth and may become outdated between publication intervals.

OCSP allows a client to request the status of one specific certificate from an Online Responder. This requires less data per request and can provide faster, more targeted status information. However, OCSP depends on network availability and the availability of the responder. In this lab environment, the Online Responder also depended on current CRL information, so its answers were only as accurate as the revocation data available to it.

### 2. What are the purposes of the OCSP Signing EKU and the OCSP NoCheck extension?

The OCSP Signing EKU uses OID `1.3.6.1.5.5.7.3.9`. It authorizes a certificate to digitally sign OCSP responses on behalf of a certification authority.

The OCSP NoCheck extension uses OID `1.3.6.1.5.5.7.48.1.5`. It tells clients that they do not need to perform a revocation check on the OCSP responder’s signing certificate. Without this extension, a circular validation problem could occur because the client might attempt to ask the OCSP responder to verify the certificate that the responder itself uses to sign responses.

### 3. What could happen if an Online Responder used stale CRL information?

If the Online Responder used stale CRL information, it might not know that a recently revoked certificate had been revoked. It could incorrectly report the certificate as Good or fail to provide a reliable response. The responder must have access to current revocation data to produce accurate certificate status responses.

This issue occurred during the lab because the responder encountered stale or unusable revocation information. Publishing a current CRL to the correct Active Directory CDP location restored the responder to a green Working state.

### 4. What happens when an organization uses a hard-fail OCSP policy?

Under a hard-fail OCSP policy, certificate validation fails when the client cannot contact the Online Responder or cannot obtain a valid response. This provides stronger revocation enforcement because an unknown status is not treated as acceptable.

The disadvantage is that valid certificates may also be rejected during a network outage, responder outage, DNS failure, firewall problem, or other temporary communication issue.

### 5. Why can OCSP provide better status information than a traditional CRL?

OCSP checks the status of one certificate through a responder instead of requiring the client to download and process an entire revocation list. This can reduce bandwidth usage and provide faster status checks.

However, OCSP does not automatically guarantee information that is newer than the underlying CRL. In this lab, the Online Responder used CRL information as its revocation provider. Therefore, the responder’s accuracy depended on the availability and freshness of the CRL.

---

## Part G — Lab Results Summary

| Test | Result |
|---|---|
| Online Responder service running | Passed |
| Revocation configuration created | Passed |
| Revocation provider operational | Passed |
| Online Responder status green | Passed |
| OCSP signing certificate present | Passed |
| Expired signing certificate renewed | Passed |
| Renewed certificate valid through 2027 | Passed |
| OCSP Signing EKU verified | Passed |
| OCSP NoCheck extension verified | Passed |
| Valid certificate AIA retrieval | Verified |
| Revoked certificate detected | Passed |
| Revocation error returned | `CRYPT_E_REVOKED` |
| Certificate AIA inspected | Passed |
| OCSP URL found in valid certificate | No — only CA Issuers LDAP AIA was present |

---

## Key Findings

1. The Online Responder service was successfully configured on PKI-SRV01.
2. The responder initially failed because its revocation provider could not obtain usable current CRL information.
3. Publishing the current CRL to the correct Active Directory CDP location restored the responder.
4. The original OCSP signing certificate had expired and had to be renewed.
5. The renewed signing certificate contained the OCSP Signing EKU and OCSP NoCheck extension.
6. The revoked PKI Admin certificate was correctly identified as revoked.
7. The valid web-server certificate contained a CA Issuers LDAP AIA entry but did not contain a direct OCSP URL.

---

## Challenges and Troubleshooting

The largest issue was the red Online Responder status. The base CRL was current, but an expired delta CRL and incorrect or incomplete CRL publication prevented the responder from accessing usable revocation information.

The issue was resolved by disabling delta CRL publication, publishing a new CRL, publishing the current CRL into the appropriate Active Directory CDP object, restarting Certificate Services and the Online Responder service, and refreshing the Online Responder console.

A second issue involved an expired OCSP response-signing certificate. A renewed certificate was enrolled from the `OCSPResponseSigning` template, and the new certificate was valid through 2027.

The final certificate inspection showed that the valid web-server certificate did not contain a direct OCSP AIA URL. Therefore, the URL Retrieval Tool verified the CA Issuers certificate location rather than displaying a direct OCSP status result.

---

## Conclusion

The Microsoft Online Responder was successfully configured and restored to an operational green Working state. The OCSP response-signing certificate was renewed, current revocation information was published, and the revoked PKI Admin certificate was correctly detected as revoked.

This lab demonstrated that an Online Responder depends on correctly configured certificate templates, valid signing certificates, current CRLs, functioning CDP locations, and proper AIA configuration. It also demonstrated how stale or incorrectly published revocation information can prevent an OCSP responder from functioning correctly.

---



