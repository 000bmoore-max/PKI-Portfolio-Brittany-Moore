# Lab 02 — AD CS Console Exploration & CA Hierarchy Documentation

**Brittany Moore**  
**May 24, 2026 :**  
**Phase:** 2 | **Week:** 9  
**Submission Path:** labs/week-09/lab-02-environment-documentation.md

---

## Part A — AD CS Console Exploration (PKI-SRV01)

certsrv.msc console nodes table:

| Node                  | Contents / Observations |
|-----------------------|-------------------------|
| Revoked Certificates  | No revoked certificates currently present in the environment |
| Issued Certificates   | Displays certificates issued by the enterprise CA |
| Pending Requests      | No pending certificate requests currently visible |
| Failed Requests       | No failed enrollment requests currently visible |
| Certificate Templates | Displays templates published to the CA |

- CA Properties — CDP path:
  ```
C:\Windows\System32\CertSrv\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl
  ```
- CA Properties — AIA path:
  ```ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services,CN=Services,<ConfigurationContainer><CAObjectClass>
  ```
- CA Properties — Database path:
  ```
  C:\Windows\system32\CertLog
  ```
- Templates visible in certtmpl.msc:
- User, Computer, Web Server, SubCA, Domain Controller, and Kerberos Authentication templates were visible in certtmpl.msc.


## Part B — CA Hierarchy Verification

Command: `certutil -store -enterprise Root`

Output:
```
================ Certificate 0 ================
Serial Number: 26373e51a6ab669340c47caef2232ce1
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 6:15 PM
 NotAfter: 4/25/2046 6:25 PM
Subject: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Signature matches Public Key
Root Certificate: Subject matches Issuer
Cert Hash(sha1): b805e6ab548f6e7c57d3989f61de7fe6a51031d1
No key provider information
Cannot find the certificate and private key for decryption.
CertUtil: -store command completed successfully.
PS C:\Users\pki.admin> certutil -store -enterprise CA
CA "Intermediate Certification Authorities"
```

Command: `certutil -store -enterprise CA`

Output:
```
CertUtil: -store command FAILED: 0x80070002 (WIN32: 2 ERROR_FILE_NOT_FOUND)
CertUtil: The system cannot find the file specified.
PS C:\Users\pki.admin> certutil -store -enterprise CA
CA "Intermediate Certification Authorities"
================ Certificate 0 ================
Serial Number: 5800000002f7714edc7f317c46000000000002
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 7:26 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Certificate Template Name (Certificate Type): SubCA
Non-root Certificate
Template: SubCA
Cert Hash(sha1): 5137a597de2c3085ec5816c7f11edc18cfcdbaf8
No key provider information
Missing stored keyset
CertUtil: -store command completed successfully.
PS C:\Users\pki.admin>
```

Observations — what Subject, Issuer, and Thumbprint confirm:
The Subject and Issuer fields confirmed the relationship between the CVI Root CA and the CVI Issuing CA 1 within the enterprise PKI heirchy. The thumbprint uniquely identified and CA certificate and verified that the trust chain was successfully installed and recognized within the Active Directory Certificate Services.

---

## Part C — Active Directory Documentation

- PKI Admins OU location: (describe here)
- pki.admin group memberships: Located within the corp.cvilab.local Active Directory domain structure under the PKI Amin organization unit.
- Domain Admins
- Domain Users
- PKI Admins
- 
- cert.manager group memberships: 
-Domain Users
-PKI Admins 
- Domain-joined computers visible:
- PKI-SRV01  
- certtmpl.msc on DC01 — same templates as PKI-SRV01: Yes 

---

## Part D — Environment Summary

Write in prose paragraphs covering:

## Part D — Environment Summary

### 1. Environment Topology
The environment consists of a Windows Active Directory domain with multiple domain-joined systems, including a domain controller and an issuing certificate authority server. Administrative services are managed through Server Manager, Active Directory Users and Computers, and Active Directory Certificate Services consoles.

### 2. CA Hierarchy
The PKI environment uses a two-tier certificate authority hierarchy consisting of a Root CA and an issuing enterprise CA named CVI Issuing CA 1. The issuing CA distributes and manages certificates within the Active Directory environment.

### 3. Certificate Templates
Certificate templates were visible through the Certificate Templates console and are used to standardize certificate enrollment and issuance policies throughout the environment.

### 4. Active Directory Structure
The Active Directory environment contains organizational units, administrative security groups, and domain-joined systems. Accounts such as pki.admin and cert.manager were assigned group memberships related to PKI administration and enterprise certificate management.

### 5. One thing you found interesting or unexpected
One interesting aspect of the lab was seeing how closely Active Directory and PKI services integrate together within enterprise infrastructure. The relationship between certificate services, group memberships, domain trust, and server roles became much clearer during the environment exploration process.

*This section is referenced in the Week 13 backup and recovery lab — write it as a runbook entry.*
