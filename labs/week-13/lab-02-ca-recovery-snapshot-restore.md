# Lab 02 — CA Recovery Simulation: Snapshot Restore

**Student Name:** Brittany Moore

**Date Completed:** July 19, 2026

**Phase:** 2 | **Week:** 13

**Submission Path:** `labs/week-13/lab-02-ca-recovery-simulation.md`

---

# Pre-Lab Verification

- Lab 01 backup files present in C:\CABackup: ☑ Yes

### certutil -ping output:

```text
Connecting to PKI-SRV01.corp.cvilab.local\CVI Issuing CA 1 ...
Server "CVI Issuing CA 1" ICertRequest2 interface is alive.
CertUtil: -ping command completed successfully.
```

### certutil -CRL output:

```text
CertUtil: -CRL command completed successfully.
```

---

# Part A — Pre-Failure Snapshot

### Pre-snapshot CA state

- CA service operational
- CRL publication successful
- Last issued Request ID: **13**

### Hypervisor

- ☑ VirtualBox
- ☐ UTM

### Snapshot name

```text
Week 13 Lab 02 - Pre-Failure CA State
```

### Snapshot taken successfully

- ☑ Yes

---

# Part B — Destructive Operation

### Option chosen

- ☑ Delete database files
- ☐ Rename CertLog folder

### Commands Used

```powershell
Get-Service CertSvc

Get-ChildItem "C:\Windows\System32\CertLog"

Remove-Item "C:\Windows\System32\CertLog\*.edb" -Force

Remove-Item "C:\Windows\System32\CertLog\*.log" -Force

Start-Service CertSvc
```

### Get-Service CertSvc (failed state)

```text
Status : Stopped

Name : CertSvc

DisplayName : Active Directory Certificate Services
```

### Event Log Errors

```text
Event ID: 17

Provider:
Microsoft-Windows-CertificationAuthority

Message:
Active Directory Certificate Services did not start.

Unable to initialize the database connection for
CVI Issuing CA 1.

File not found

0xC8000713

ESE: -1811 JET_errFileNotFound
```

### Failure description

Deleting the CA database (.edb) and log files prevented the
Certification Authority service from locating its database.

When the service attempted to start, it immediately failed because the
database required for certificate operations no longer existed.

This accurately simulated a catastrophic CA database loss.

---

# Part C — Snapshot Restore

### PKI-SRV01 powered off before restore

- ☑ Yes

### Snapshot restore completed

- ☑ Yes

### VM started and login successful

- ☑ Yes

---

# Part D — Post-Recovery Verification

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
ICertRequest2 interface is alive (47 ms)

CertUtil:
-ping command completed successfully.
```

### certutil -CRL

```text
CertUtil:
-CRL command completed successfully.
```

### Issued Request IDs

```text
3
4
6
9
10
11
12
13
```

### Highest Request ID

```text
13
```

### CRL accessible at HTTP CDP

- ☒ No

Reason:

```text
The restored snapshot did not contain the
C:\inetpub\wwwroot\CertEnroll
directory.

Browsing to the HTTP CDP returned
HTTP 404 Not Found.
```

### Issued certificate count matches pre-failure baseline

- ☑ Yes

### Event log clean

- ☑ Yes

No new Certification Authority errors were present after the snapshot
restore.

### Recovery completed at

```text
Sunday, July 19, 2026
10:15:47 PM
```

### Time from snapshot restore to CA fully operational

```text
Approximately 10 minutes.
```

---

# Part E — Lab Report

## 1. Describe the failure state.

After deleting the CA database files and transaction logs, the
Certification Authority service failed to start.

Get-Service showed the CertSvc service stopped.

The Application Event Log recorded Event ID 17 stating that Active
Directory Certificate Services could not initialize the database because
the database file was missing (JET_errFileNotFound).

A real administrator would immediately recognize this as a catastrophic
database failure requiring restoration from backup or snapshot.

---

## 2. Walk through the snapshot restore procedure.

A VirtualBox snapshot named **Week 13 Lab 02 - Pre-Failure CA State**
was created before the destructive operation.

After simulating database loss, the virtual machine was powered off.

The VirtualBox snapshot was restored, returning the entire virtual
machine—including the operating system, registry, Active Directory
Certificate Services configuration, database, transaction logs, and disk
state—to the exact point when the snapshot was taken.

After the VM restarted, the CA service returned to normal operation.

---

## 3. Walk through post-recovery verification.

Verifying only that the CertSvc service is running is not sufficient.

A complete recovery verification also includes:

- Confirming the service is running.
- Verifying certificate requests can be processed using `certutil -ping`.
- Publishing a CRL successfully using `certutil -CRL`.
- Confirming issued certificates still exist.
- Reviewing Event Viewer for any remaining Certification Authority errors.
- Verifying CRL publication locations when applicable.

These checks confirm that the CA is actually functioning rather than
simply running.

---

## 4. If the last snapshot was 72 hours old in production, what data would be lost?

Any certificates issued, revoked, renewed, or modified after the
snapshot was taken would be lost.

The CA database would return to the earlier snapshot state, resulting in
missing Request IDs, certificate history, revocation information, and
other PKI records.

This is why production environments require frequent backups and clearly
defined Recovery Point Objectives (RPOs).

---

## 5. Compare snapshot restore to file-based restore.

Snapshot restoration is extremely fast because it restores the entire
virtual machine to a known-good state.

However, snapshots are only available in virtualized environments and
should not replace traditional backups.

File-based backups created with tools such as `certutil -backup` and
Windows Server Backup can be restored onto new hardware or rebuilt
servers, making them appropriate for long-term disaster recovery.

---

# Submission Checklist

- ☑ Logged in as CORP\pki.admin

- ☑ Lab 01 backup files confirmed present before snapshot

- ☑ Pre-failure CA state recorded

- ☑ Snapshot taken — name documented

- ☑ Destructive operation performed — failure state documented

- ☑ Snapshot restore completed without errors

- ☑ Post-recovery: certutil -ping successful

- ☑ Post-recovery: certutil -CRL successful

- ☑ Post-recovery: issued certificate count matches baseline

- ☑ Post-recovery: event log reviewed

- ☑ Recovery time recorded

- ☑ All five lab report questions answered

---
