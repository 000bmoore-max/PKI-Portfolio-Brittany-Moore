# Lab 01 — Full CA Backup: Database, Keys, and System State

**Student Name:** Brittany Moore

**Date Completed:** July 18, 2026

**Phase:** 2 | **Week:** 13

**Submission Path:** `labs/week-13/lab-01-ca-backup.md`

---

## Pre-Lab Verification

### whoami output

```text
corp\pki.admin
```

### Get-Service CertSvc output

```text
Status   Name      DisplayName
------   ----      -----------
Running  CertSvc   Active Directory Certificate Services
```

### certutil -ping output

```text
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1 ...
Server "CVI Issuing CA 1" ICertRequest2 interface is alive (31ms)
CertUtil: -ping command completed successfully.
```

### Pre-Lab Observation

The lab was performed on `PKI-SRV01` while logged in as `CORP\pki.admin`. Active Directory Certificate Services was running, and the `CVI Issuing CA 1` certification authority responded successfully to the `certutil -ping` request.

---

## Part A — Backup Folder

### C:\CABackup created via File Explorer

- [x] Yes

The `C:\CABackup` folder was created manually through File Explorer as required.

### dir C:\CABackup

```powershell
dir C:\CABackup
```

### Result

```text
No files or subfolders were displayed.
```

### Observation

The command returned a blank listing, confirming that `C:\CABackup` existed and was empty before the CA backup was started.

---

## Part B — CA Database and Private Key Backup

### Backup Command

```powershell
certutil -backup -p "<password omitted>" "C:\CABackup"
```

The password used to protect the CA private-key backup has been intentionally omitted from this report.

### certutil -backup -p output

```text
Backing up CA database and private key for:

PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1

The certification authority database was backed up successfully.

The certification authority private key and certificate were exported successfully.

The database was successfully truncated.

CertUtil: -backup command completed successfully.
```

### Backup File Verification Command

```powershell
Get-ChildItem "C:\CABackup" -Recurse |
Select-Object FullName, Length, LastWriteTime
```

### Backup File Listing

```text
FullName                                           Length   LastWriteTime
--------                                           ------   -------------
C:\CABackup\DataBase                                         7/18/2026 9:58:12 AM
C:\CABackup\CVI Issuing CA 1.p12                  4631       7/18/2026 9:58:12 AM
C:\CABackup\DataBase\certbkxp.dat                 398        7/18/2026 9:58:12 AM
C:\CABackup\DataBase\CVI Issuing CA 1.edb         1052672    7/18/2026 9:58:12 AM
C:\CABackup\DataBase\edb00003.log                 1048576    7/18/2026 9:58:12 AM
```

### .p12 File

- **Full path:** `C:\CABackup\CVI Issuing CA 1.p12`
- **Size:** `4,631 bytes`
- **Timestamp:** `7/18/2026 9:58:12 AM`

### .edb File

- **Full path:** `C:\CABackup\DataBase\CVI Issuing CA 1.edb`
- **Size:** `1,052,672 bytes`
- **Timestamp:** `7/18/2026 9:58:12 AM`

### Additional Database Backup Files

| File | Size | Timestamp |
|---|---:|---|
| `C:\CABackup\DataBase\certbkxp.dat` | 398 bytes | 7/18/2026 9:58:12 AM |
| `C:\CABackup\DataBase\edb00003.log` | 1,048,576 bytes | 7/18/2026 9:58:12 AM |

### certutil -dump Command

```powershell
$p12 = Get-ChildItem "C:\CABackup" -Recurse -Filter *.p12 |
Select-Object -First 1

certutil -dump $p12.FullName |
Select-Object -First 20
```

### certutil -dump Output — First 20 Lines

```text
Enter PFX password:

================ Certificate 0 ================
================ Begin Nesting Level 1 ================

Element 0:

Serial Number: 5800000002f7714edc7f3317c4600000000000002
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
NotBefore: 4/25/2026 7:26 PM
NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Certificate Template Name (Certificate Type): SubCA
Non-root Certificate
Template: SubCA, Subordinate Certification Authority
Cert Hash(sha1): 5137a597de2c3085ec5816c7f11edc18cfcdbaf8
================ End Nesting Level 1 ================
Provider = Microsoft Software Key Storage Provider
Private key is NOT plain text exportable
Encryption test passed
```

### Backup Verification

The `.p12` file was successfully opened with `certutil -dump` using the backup password. The output confirmed that the file contained the certificate and private key for `CVI Issuing CA 1`.

The following properties were verified:

- Issuer: `CVI Root CA`
- Subject: `CVI Issuing CA 1`
- Template: `SubCA`
- Provider: `Microsoft Software Key Storage Provider`
- Private key not plain-text exportable
- Encryption test passed

### Password Storage Location

The password was memorized by the student and was not stored inside `C:\CABackup`.

The actual password is not included in this report.

### Security Observation

Storing the password inside the same folder as the encrypted `.p12` file would create a serious security risk. Anyone who gained access to the folder would possess both the protected private-key backup and the password required to decrypt it.

---

## Part C — Windows System State Backup

### Windows Server Backup Feature Check

```powershell
Get-WindowsFeature Windows-Server-Backup
```

### Initial Result

```text
[ ] Windows Server Backup    Windows-Server-Backup    Available
```

The Windows Server Backup feature was available but was not initially installed.

### Installation Command

```powershell
Install-WindowsFeature Windows-Server-Backup -IncludeManagementTools
```

### Installation Output

```text
Success Restart Needed Exit Code       Feature Result
------- -------------- ---------       --------------
True    No             NoChangeNeeded  {}
```

### Post-Installation Verification

```powershell
Get-WindowsFeature Windows-Server-Backup
```

### Result

```text
[X] Windows Server Backup    Windows-Server-Backup    Installed
```

### Volume Inspection Command

```powershell
Get-Volume |
Select-Object DriveLetter, FileSystemLabel, FileSystem, SizeRemaining, Size
```

### Relevant Results

```text
DriveLetter    : C
FileSystem     : NTFS
SizeRemaining  : 52947517440
Size           : 63767048192

DriveLetter    : D
FileSystem     :
SizeRemaining  : 0
Size           : 0
```

The `C:` drive was the only usable hard-drive volume. The `D:` drive was a virtual DVD or removable-media drive with no usable capacity.

### System State Backup Command

```powershell
wbadmin start systemstatebackup -backupTarget:D: -quiet
```

### wbadmin Output

```text
wbadmin 1.0 - Backup command-line tool
(C) Copyright Microsoft Corporation. All rights reserved.

Starting to back up the system state [7/18/2026 11:01 AM]...
Retrieving volume information...

This will back up the system state from volume(s)
System Reserved (100.00 MB), (C:), and associated system volumes to D:.

You cannot save a system state backup to DVDs or other removable disks.
```

### Documented Limitation

The system-state backup could not be completed because the virtual lab did not contain a separate usable fixed disk or network backup destination.

The only destination other than the system drive was `D:`, which was recognized as a DVD or removable-media drive. Windows Server Backup does not allow system-state backups to be stored on DVD or other removable-media devices.

The failure was therefore caused by a limitation of the virtual lab environment rather than a failure of Windows Server Backup or Active Directory Certificate Services.

### wbadmin get versions Command

```powershell
wbadmin get versions
```

### Output

```text
wbadmin 1.0 - Backup command-line tool
(C) Copyright Microsoft Corporation. All rights reserved.

ERROR - No backup was found.
```

### Observation

No Windows system-state backup version was listed because the attempted backup could not be written to the `D:` destination.

---

## Part D — Post-Backup Verification

### Get-Service CertSvc

```powershell
Get-Service CertSvc
```

### Output

```text
Status   Name      DisplayName
------   ----      -----------
Running  CertSvc   Active Directory Certificate Services
```

### certutil -ping

```powershell
certutil -ping
```

### Output

```text
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1 ...
Server "CVI Issuing CA 1" ICertRequest2 interface is alive (16ms)
CertUtil: -ping command completed successfully.
```

### certutil -CRL

```powershell
certutil -CRL
```

### Output

```text
CertUtil: -CRL command completed successfully.
```

### Post-Backup Observation

The Active Directory Certificate Services service remained in the `Running` state after the backup operation.

The CA responded successfully to `certutil -ping`, and the `certutil -CRL` command completed successfully. These results confirmed that the CA remained operational and could continue publishing revocation information after the backup.

---

## Part E — Lab Report

### 1. Describe the three backup components and what recovery requires if each is missing.

A complete certification authority recovery plan contains three primary components: the CA database, the CA certificate and private key, and the Windows system state.

The CA database contains issued-certificate records, pending requests, revoked-certificate information, certificate serial numbers, and the CA transaction history. If the database is missing, the CA may still possess its identity and private key, but it would lose its record of previously issued and revoked certificates. This could prevent proper certificate management and revocation processing.

The `.p12` file contains the CA certificate and its private key. The private key is the most critical cryptographic component because it allows the CA to sign certificates and certificate revocation lists. If the private key is missing, the restored server cannot continue functioning as the same CA. A replacement CA would have a different key and identity, and certificates issued by the original CA could not be managed or re-signed by the replacement.

The Windows system-state backup contains operating-system and role-specific configuration that `certutil -backup` does not capture. This can include the registry, boot files, COM+ registration data, system files, Active Directory-related configuration, and AD CS registry settings. If the system state is missing, the database and private key may still exist, but the administrator may have to reconstruct server settings, registry configuration, service configuration, and other dependencies manually.

A successful full recovery requires all three components whenever possible.

### 2. Explain what VSS does and why it is better than stopping the CA service.

Volume Shadow Copy Service, or VSS, creates a consistent point-in-time snapshot of files that may still be open or in use by the operating system and applications.

VSS coordinates with compatible applications and system components so that data can be captured in a consistent state without requiring the application service to remain offline for the entire backup process.

Using VSS is better than manually stopping the CA service because it reduces service disruption. Stopping Active Directory Certificate Services prevents certificate enrollment, certificate issuance, revocation processing, and CRL publication while the service is offline.

A VSS-based backup can protect active files while allowing the CA to remain available. It also reduces the chance of administrator error caused by forgetting to restart the CA service after a manual shutdown.

### 3. Where did you store the password, and why is storing it in C:\CABackup a security problem?

The backup password was memorized and was not stored inside `C:\CABackup`.

The password is required to decrypt and restore the CA private key contained in the `.p12` file. Storing the password in the same folder as the encrypted backup would defeat much of the protection provided by encryption.

If an attacker copied `C:\CABackup` and the password was stored in that same folder, the attacker could decrypt the CA private key. Possession of the CA private key could allow the attacker to impersonate the certification authority, issue fraudulent certificates, and sign malicious content that appears trusted.

In a production environment, the password should be stored separately in an approved enterprise password manager, hardware vault, or secured offline record with restricted access.

### 4. What does the system state backup capture that certutil -backup does not?

The `certutil -backup` command primarily protects the CA database, CA certificate, private key, database logs, and associated CA backup files.

A Windows system-state backup protects broader server configuration and operating-system information. Depending on the server roles installed, system state can include:

- The Windows registry
- Boot files
- Protected system files
- COM+ class registration database
- Active Directory Domain Services data
- SYSVOL
- Certificate Services configuration stored in the registry
- Other role-specific configuration
- System service configuration

This information is important because a restored CA needs more than its database and private key. It also needs the correct service configuration, registry settings, operating-system components, and server-role configuration to operate in the same manner as the original system.

### 5. Explain the relationship between backup frequency and RPO.

Recovery Point Objective, or RPO, defines the maximum acceptable amount of data loss measured in time.

Backup frequency directly affects the achievable RPO. If the CA database is backed up once every seven days, a disaster could result in the loss of up to seven days of certificate issuance, revocation, and request-history records.

If the CA is backed up daily, the maximum expected data loss is reduced to approximately one day. More frequent backups create a smaller RPO and reduce the number of CA transactions that may need to be reconstructed after a failure.

The required backup frequency should be based on how active and critical the CA is. A high-volume production CA may require daily or more frequent backups, while a low-volume offline root CA may use a less frequent schedule because it is rarely powered on or used.

---

## Submission Checklist

- [x] Logged in as `CORP\pki.admin`
- [x] `C:\CABackup` created via File Explorer and confirmed empty
- [x] `certutil -backup -p` completed
- [x] Backup output documented
- [x] `.p12` and `.edb` files confirmed
- [x] File sizes and timestamps recorded
- [x] `certutil -dump` confirmed that the `.p12` file was readable
- [x] Password stored separately from `C:\CABackup`
- [x] Windows Server Backup installed
- [x] `wbadmin` limitation documented
- [x] `wbadmin get versions` output recorded
- [x] CA operational after backup
- [x] `Get-Service CertSvc` succeeded
- [x] `certutil -ping` succeeded
- [x] `certutil -CRL` succeeded
- [x] All five lab report questions answered

---

## Challenges and Troubleshooting

### Incorrect Backup Command Syntax

The first backup attempt used the placeholder text `YOURPASSWORD` and contained incorrect path spacing. This caused errors including:

```text
ERROR_INVALID_PARAMETER
The parameter is incorrect.
```

and:

```text
ERROR_FILE_EXISTS
The file exists.
```

The issue was corrected by selecting a new backup password, recording it separately, and entering the path correctly without a space:

```powershell
certutil -backup -p "<password omitted>" "C:\CABackup"
```

### Hidden PFX Password Entry

When `certutil -dump` requested the PFX password, the password entry was not visually displayed. No characters, dots, or asterisks appeared while typing.

The password had to be entered while the prompt appeared blank and then submitted by pressing Enter once. After the correct password was entered, the certificate and private-key information were displayed, and the encryption test passed.

### Virtual Machine Restart

The virtual machine restarted during the lab. The previously created backup files remained available in `C:\CABackup`, so the entire backup did not need to be repeated.

The file list and `.p12` contents were reverified after the restart.

### Missing System-State Destination

The system-state backup could not be completed because the lab virtual machine did not have a separate fixed backup disk.

The `D:` drive was a DVD or removable-media drive, and `wbadmin` returned:

```text
You cannot save a system state backup to DVDs or other removable disks.
```

This limitation was documented as permitted by the lab instructions.

---

## Conclusion

The CA database, database logs, certificate, and private key for `CVI Issuing CA 1` were successfully backed up to `C:\CABackup`.

The `.p12` file was verified as readable and contained the `CVI Issuing CA 1` subordinate CA certificate and protected private key. The CA database `.edb` file and transaction log were also present with valid timestamps and file sizes.

Windows Server Backup was successfully installed. A Windows system-state backup could not be completed because the virtual lab did not provide a separate fixed-disk or network backup destination. The limitation and `wbadmin` output were documented.

Post-backup tests confirmed that Active Directory Certificate Services remained running, the CA interface remained reachable, and CRL publication continued to function.

---

## Git Submission

Save this completed file as:

```text
labs/week-13/lab-01-ca-backup.md
```

Run:

```bash
git add labs/week-13/lab-01-ca-backup.md
git commit -m "Week 13 Lab 01 — Full CA Backup: database, private key, system state"
git push
```

