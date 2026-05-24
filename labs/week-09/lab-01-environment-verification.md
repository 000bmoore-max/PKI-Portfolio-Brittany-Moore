# Lab 01 — Environment Verification & VM Connectivity Check

**Student Name:** Brittany Moore  
**Date Completed:** May 24, 2026  
**Phase:** 2 | **Week:** 9  
**Submission Path:** labs/week-09/lab-01-environment-verification.md

---

## Step 1 — VM Startup & Login

VMs started in correct order (DC01 → PKI-SRV01): Yes

Login credentials table:

| VM         | Account Used        | Login Successful? |
|------------|---------------------|-------------------|
| DC01       | CORP\pki.admin      | Yes               |
| PKI-SRV01  | CORP\pki.admin      | Yes               |
| Root-CA    | .\Administrator     | Yes               |

Notes / issues encountered:

DC01 was started first and allowed to fully initialize before starting PKI-SRV01. The environment loaded successfully and authentication completed without major issues. Initial startup required patience while services initialized.

---

## Step 2 — VM Connectivity Test

Command: `Test-Connection -ComputerName DC01 -Count 2`

Output:

```powershell

PS C:\Users\pki.admin> Test-Connection -ComputerName DC01 -Count 2

Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms) 
------        -----------     -----------      -----------                              -----    -------- 
PKI-SRV01     DC01            192.168.10.10                                             32       1        
PKI-SRV01     DC01            192.168.10.10                                             32       0        
```

DC01 responded successfully: Yes

---

## Step 3 — CertSvc Service Status

Command: `Get-Service -Name CertSvc`

Output:

```powershell
Status   Name               DisplayName
------   ----               -----------
Running  CertSvc            Active Directory Certificate Services
```

Status confirmed as Running: Yes

---

## Step 4 — certsrv.msc Console Verification

- CVI Issuing CA 1 visible: Yes
- Console icon status (green/red): Green
- Nodes visible:
  - Revoked Certificates
  - Issued Certificates
  - Pending Requests
  - Failed Requests
  - Certificate Templates

---

## Step 5 — CertLog Folder Contents

Command: `Get-ChildItem "C:\Windows\System32\CertLog"`

Output:

## Step 5 — CertLog Folder Contents

Command: `Get-ChildItem "C:\Windows\System32\CertLog"`

Output:

```powershell
Get-ChildItem : Access to the path 'C:\Windows\System32\CertLog' is denied.
CategoryInfo          : PermissionDenied
FullyQualifiedErrorId : DirUnauthorizedAccessError

The command successfully reached the CertLog directory path, but the current account did not have sufficient permissions to enumerate the folder contents.
```

---

## Reflection

- One thing that went well:
  The VM startup process and CA service verification completed successfully. I was able to confirm connectivity between systems and verify that Active Directory Certificate Services was operational.

- One thing that was confusing or unexpected:
  Understanding the relationship between the offline Root CA and the online Issuing CA was initially confusing. It became clearer after reviewing the two-tier PKI architecture and understanding why the Root CA remains offline for security purposes.

