# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
In this lab, I investigated how to verify the revocation status of a digital certificate using OCSP (Online Certificate Status Protocol). The goal was to determine whether a certificate is still valid or has been revoked by the Certificate Authority. This lab focused on real-time certificate validation, which is an important concept in PKI systems. Understanding this process helps ensure secure communication by confirming trust in certificates.

## Environment
- Operating System: Windows 10  
- Terminal Used: Windows PowerShell  
- OpenSSL Version (openssl version): OpenSSL 3.x (Win64)  
- Website Used for Certificate Retrieval: https://www.google.com  

## Steps Performed
1. Navigated to the working directory in PowerShell where certificate files were stored.  
2. Verified that the leaf and issuer certificates were in the correct `.pem` format.  
3. Identified the OCSP URL from the certificate’s Authority Information Access (AIA) extension.  
4. Executed the OpenSSL OCSP command using the issuer and leaf certificate.  
5. Redirected the output into a file (`ocsp_response.txt`) and reviewed the results.  

## Results
- The OCSP URL found in the certificate was: http://ocsp.pki.goog/gts1o1  
- The OCSP response status was **good**, meaning the certificate has not been revoked and is still trusted.  
- The “This Update” value showed when the OCSP response was generated, and the “Next Update” value indicated when the status should be checked again.  
- The CRL Distribution Point was located in the certificate’s extensions and provides a backup method for checking revocation status if OCSP is unavailable.  

## Key Findings
The OCSP response confirmed that the certificate is valid and has not been revoked. This demonstrates how OCSP provides real-time validation compared to static CRL methods. It also showed how certificate extensions contain important validation endpoints such as OCSP and CRL URLs. These findings highlight the importance of proper certificate verification in secure communications.

## Explanation
- OCSP vs CRL: OCSP provides real-time revocation status by querying a responder, while CRLs require downloading a full list of revoked certificates. OCSP is faster and more efficient.  
- OCSP requires both the leaf and issuer certificate because the issuer is needed to verify the certificate’s signature and trust chain.  
- A certificate may show “unknown” status if the OCSP responder does not have information about that certificate or if the request cannot be validated.  

## Challenges / Troubleshooting
During the lab, I encountered issues with incorrect file extensions where `.pem` files were mistakenly saved as `.txt`. This prevented OpenSSL from reading the certificates. I also experienced an issue where PowerShell did not recognize the `openssl` command, which required using the full file path. Additionally, I had to resolve Git errors when pushing my work to GitHub.  

## Artifacts
- leaf_cert_w5.pem  
- issuer_cert_w5.pem  
- ocsp_response.txt  
- google_output.txt  
