# 1 Lab — [Week 2 lab]
 
## Overview
The purpose of this lab is to provide a hands on demonstrations of how to utilize Powershell and SSL to create folders, encrypt and decrypt information. Upon inputting the correct commands, this app shows how text is encrypted and decrypted allowing us to view the message in plaint text and encrypted form. It also teaches how to check for the changes that were made.

 The PKI concept or system behavior that I was investigating was  encryption and decryption
 
---
 
## Environment
Document the environment used to complete the lab.
 
- Operating System: Windows 
- Terminal Used:
- OpenSSL Version 32
 
---
 
## Steps Performed
Summarize the key steps you performed to complete the lab.
 
Do **not copy the lab instructions**.
Describe what you actually did.
 
1.Created an artifact directory using the command: mkdir -p labs/02-week-02-cryptography-fundamentals/submissions/encrypted
2.Created a plain text file and confirmed that the file was in the file directory. 
3.Encrypted the file and checked to see that the file was decrypted. 

 
---
 
## Results
Include the important outputs or findings from the lab.
 
Examples may include:
 
- command outputs- I used a combination of SSL and Windows Powershell in order to 
- certificate fields
- verification results
- screenshots (if applicable)
 
If you include screenshots, store them in the **assets folder** and reference them here.
 
Example:
 
![Certificate Output](assets/certificate-output.png)
 
---
 
## Key Findings
Document the most important observations from the lab.
 
Examples:
 
- Certificate issuer
- Public key algorithm used
- Certificate extensions present
- Trust chain relationships
- Validation results
 
•
•
•
 
---
 
## Explanation
Explain **why the results matter**.
 
Examples:
 
- Why the issuer is important in PKI
- Why SAN is required for modern TLS validation
- Why the certificate chain validates successfully
- Why a misconfiguration would cause a failure
 
---
 
## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.
 
Examples:
 
- command errors
- missing intermediate certificates
- verification failures
 
---
 
## Artifacts
List the files generated during this lab.
 
Examples:
 
- leaf_cert.pem
- server.pem
- intermediate.pem
- root.pem
- screenshots stored in assets/
<img width="596" height="1881" alt="image" src="https://github.com/user-attachments/assets/f7f83777-a7ea-4052-a52e-bc62a33592dc" />
