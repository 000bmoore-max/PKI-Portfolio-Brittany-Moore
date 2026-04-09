# Lab 01 — Generate a CSR

## Overview
This lab focused on generating a Certificate Signing Request (CSR) using OpenSSL and understanding the process of requesting a digital certificate. The main PKI concept explored was how a private key is used to create a CSR, which is then submitted to a Certificate Authority (CA) for validation and certificate issuance.

## Environment
- Operating System: Windows
- Terminal Used: Windows PowerShell
- OpenSSL Version: OpenSSL 3.6.1 27
## Steps Performed
1. Created a directory structure to organize lab files.
2. Generated a private key using OpenSSL.
3. Created a CSR using the private key and entered identifying information (Distinguished Name fields).
4. Self-signed the CSR to generate a certificate.
5. Verified that the CSR and certificate files were successfully created.

## Results
- The Subject fields included Country (US), State (Michigan), City (Detroit), Organization (PKI), Organizational Unit, Common Name, and Email. These fields communicate the identity of the entity requesting the certificate to the Certificate Authority.
- The `openssl req -text` command displayed details of the CSR, including the subject information, public key, and signature algorithm.
- The CSR differed from the signed certificate in that the CSR is a request containing identity and public key information, while the certificate is an issued and signed document that can be trusted.
- The diff comparison showed that the public key in the CSR matched the public key in the signed certificate, confirming that the certificate was issued from the same key pair.

## Key Findings
A CSR is a critical part of the PKI process because it securely packages identity information and a public key for submission to a Certificate Authority. The private key remains on the local machine and is never shared.

## Explanation
- The private key must never leave the requestor's machine because it is used to prove identity and establish secure communication. If compromised, it could allow attackers to impersonate the entity.
- A CSR is a request for a certificate, while a signed certificate is the validated and trusted version issued by a CA.
- Self-signing is appropriate in testing environments or internal systems, while a trusted CA should be used for public-facing services to ensure browser and user trust.

## Challenges / Troubleshooting
One challenge encountered was that OpenSSL was not initially recognized in PowerShell. This was resolved by calling the OpenSSL executable using its full file path. Additionally, proper syntax was required when executing commands in PowerShell.

## Artifacts
- test_csr.pem
- test_cert.pem
