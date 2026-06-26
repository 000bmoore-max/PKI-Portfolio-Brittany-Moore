Lab 01: Build a Certificate Profile for a Service Account

Student Name: Brittany Moore
Date Completed: June 26, 2026
Phase: 2 | Week: 11
Submission Path: labs/week-11/lab-01-service-account-profile.md

⸻

Pre-Lab Verification

If you can log into PKI-SRV01 as CORP\pki.admin, you are communicating with DC01 and the environment is ready. Proceed to Part A.

Pre-lab verification completed successfully. I logged into PKI-SRV01 as CORP\pki.admin and verified communication with DC01.

⸻

Part A — Design the CVI-ServiceAccount Template

Step 1 — Open the Certificate Templates Console

Completed successfully using certtmpl.msc.

Step 2 — Duplicate the User Template

Source template duplicated: User

Reason for choosing this source template:

The User template was selected because svc.autoenroll is an Active Directory user account rather than a computer account. The User template provides the appropriate baseline configuration for client authentication and user-based certificate enrollment. The Computer template was not appropriate because it is designed for machine identities and includes additional functionality that is not required for a service account certificate.

Step 3 — Set Compatibility Settings

Compatibility settings selected:

* Certification Authority: Windows Server 2012 R2
* Certificate Recipient: Windows 8.1 / Windows Server 2012 R2

Step 4 — Set the Template Name (General Tab)

General tab — Template names:

Field	Value
Template display name	CVI Service Account
Template name (internal)	CVIServiceAccount
Schema version	Version 4

Step 5 — Configure Key Usage

Key Usage	Included?	Reason
Digital Signature	Yes	Required for certificate-based authentication
Key Encipherment	No	The service account certificate is not used for key exchange
Data Encipherment	No	The certificate is not used for data encryption
Non-Repudiation	No	The certificate is not intended for legal proof of authorship

Explanation of Key Usage decisions:

Only Digital Signature was enabled because the service account certificate is used for authentication. Key Encipherment, Data Encipherment, and Non-Repudiation were disabled because they are not required for the service account’s intended purpose. Restricting key usage reduces unnecessary functionality and follows the principle of least privilege.

Step 6 — Configure Extended Key Usage (Application Policies)

EKU	Included?	OID	Reason
Client Authentication	Yes	1.3.6.1.5.5.7.3.2	Required for service account authentication
Server Authentication	No	1.3.6.1.5.5.7.3.1	The service account is not operating as a server
Code Signing	No	1.3.6.1.5.5.7.3.3	The certificate will not be used to sign code
Secure Email	No	1.3.6.1.5.5.7.3.4	The service account does not require email encryption or signing

Explanation of EKU decisions:

The service account certificate only requires Client Authentication because its purpose is to authenticate the identity of the svc.autoenroll account. Other EKUs, such as Secure Email, Encrypting File System, Server Authentication, and Code Signing, were removed because they are not required and would unnecessarily expand the certificate’s capabilities.

Step 7 — Configure Subject Name

Setting	Value Selected	Reason
Subject name format	User principal name (UPN)	Ensures the certificate identity matches the Active Directory account
Include this information in the subject name	Build from Active Directory information	Automatically populates the certificate subject from AD attributes

Explanation of Subject Name decision:

Build from Active Directory information was selected because the svc.autoenroll account already exists in Active Directory. This approach ensures that the certificate identity is generated from authoritative directory information rather than being manually entered, reducing the possibility of configuration errors or identity spoofing.

Step 8 — Set Validity Period

Setting	Value	Reason
Validity period	1 year	Limits the exposure window if the certificate or private key is compromised
Renewal period	6 weeks	Provides sufficient time to renew the certificate before expiration

Explanation of validity period decision:

A one-year validity period was selected to balance security and administrative overhead. Shorter certificate lifetimes reduce the risk associated with compromised credentials while still allowing practical certificate management. The default six-week renewal period provides adequate time for certificate renewal operations.

Step 9 — Set Enrollment Permissions (Security Tab)

Group / Account	Read	Enroll	Autoenroll	Reason
Authenticated Users	Yes	No	No	Allows visibility of the template without permitting enrollment
CORP\svc.autoenroll	Yes	Yes	Yes	Allows the service account to enroll and automatically renew certificates
Domain Admins	Yes	Yes	Yes	Allows administrators to manage and troubleshoot certificate enrollment

Explanation of enrollment permission decisions:

Enrollment permissions were restricted to the svc.autoenroll account to prevent unauthorized users from requesting certificates that could impersonate the service account. This restriction reduces the risk of privilege escalation, identity spoofing, and unauthorized access to services that rely on certificate-based authentication.

Step 10 — Save the Template

Template saved and visible in certtmpl.msc:

* Yes

⸻

Part B — Publish the Template and Issue the Certificate

Step 1 — Publish the Template to the CA

CVI-ServiceAccount visible in Certificate Templates node:

* Yes
* No — describe what happened:

The CVI Service Account template was successfully published to the CVI Issuing CA and appeared in the Certificate Templates node.

Step 2 — Request the Certificate for svc.autoenroll

Enrollment wizard — enrollment policy selected:

Active Directory Enrollment Policy

Templates visible in the wizard:

Basic EFS
Copy of User
CVI Service Account
User

CVI-ServiceAccount visible in wizard:

* Yes
* No — troubleshooting steps taken:

Certificate request submitted:

* Yes — issued immediately
* Yes — pending manager approval (describe resolution):
* No — error encountered:

The certificate enrollment completed successfully with a status of Succeeded.