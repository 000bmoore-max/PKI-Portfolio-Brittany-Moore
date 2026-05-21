# PKI Troubleshooting Runbook  
## Hidden Windows File Extensions Causing PEM Certificate Errors

Phase 1 · Week 5–6 · CyberVisionaries Institute

| Scenario | Trigger | Severity | Est. Resolution |
|---|---|---|---|
| PEM certificate file not recognized by OpenSSL | File saved as `.pem.txt` or `.pem.pem` | Medium — lab validation fails | 5–15 minutes |

---

# PROBLEM

A certificate file appears to be a valid PEM certificate, but OpenSSL fails to read the file correctly.

Examples of the issue:

```text
leaf_cert.pem.txt
issuer_cert.pem.pem
enterprise_cert.pem.txt
```

The file may visually appear correct in Windows File Explorer because Windows hides known file extensions by default.

This can cause:
- OpenSSL parsing failures
- invalid certificate errors
- incorrect lab submissions
- broken certificate validation

Root cause:
Windows hides the true file extension, causing users to accidentally save files with double extensions or incorrect file types.

---

# DIAGNOSIS STEPS

## 1. Navigate to the certificate file

Open File Explorer and locate the certificate file.

Example:

```text
Downloads
labs/week-05/submissions/
```

---

## 2. Enable hidden file extensions

In Windows File Explorer:

```text
View → Show → File name extensions
```

OR on older Windows versions:

```text
View → File name extensions
```

This reveals the REAL file name.

Example:

Before enabling extensions:

```text
leaf_cert.pem
```

After enabling extensions:

```text
leaf_cert.pem.txt
```

This confirms the file extension problem.

---

# 3. Verify the certificate content

Right click the file:

```text
Open With → Notepad
```

A valid PEM certificate should contain:

```text
-----BEGIN CERTIFICATE-----
```

and:

```text
-----END CERTIFICATE-----
```

If the file does not contain these lines, it may not be a valid PEM certificate.

---

# 4. Test the certificate with OpenSSL

Run the following command in PowerShell:

```powershell
& "C:\Program Files\OpenSSL-Win64\bin\openssl.exe" x509 -in leaf_cert.pem -text -noout
```

Possible error output:

```text
unable to load certificate
Expecting: TRUSTED CERTIFICATE
```

This commonly occurs when:
- the extension is incorrect
- the file is not PEM formatted
- the filename is wrong

---

# RESOLUTION

## 1. Rename the file manually in File Explorer

If the file is:

```text
leaf_cert.pem.txt
```

rename it to:

```text
leaf_cert.pem
```

If the file is:

```text
issuer_cert.pem.pem
```

rename it to:

```text
issuer_cert.pem
```

When Windows displays:

```text
If you change a file name extension, the file might become unusable.
```

Click:

```text
Yes
```

---

## 2. Verify the extension changed successfully

The file should now display as:

```text
leaf_cert.pem
```

NOT:

```text
leaf_cert.pem.txt
```

---

## 3. Retest using OpenSSL

Run:

```powershell
& "C:\Program Files\OpenSSL-Win64\bin\openssl.exe" x509 -in leaf_cert.pem -text -noout
```

If successful, OpenSSL will display:
- issuer
- subject
- validity dates
- SAN fields
- certificate details

instead of an error.

---

# NOTES & COMMON PITFALLS

- Windows hides file extensions by default.
- Notepad frequently appends `.txt` automatically.
- A PEM certificate is still a text file, but the extension matters.
- Double extensions such as `.pem.pem` may occur during manual renaming.
- OpenSSL errors are often caused by filename problems, not certificate corruption.
- Always verify both:
  - certificate contents
  - file extension

---

# PREVENTION

When saving PEM files in Notepad:

Use:

```text
Save As → Save as type: All Files
```

Then save using:

```text
certificate.pem
```

NOT:

```text
certificate.pem.txt
```

Always enable:

```text
File name extensions
```

before working with PKI labs.

---

# RELATED PHASE 1 LABS

```text
labs/week-05/submissions/revocation-status/
labs/week-06/submissions/san-mismatch/
labs/week-07/submissions/enterprise-analysis/
```
