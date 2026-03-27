# Lab — Hashing & Integrity
 
## Overview
This lab focused on understanding how cryptographic hashing works using OpenSSL. The goal was to generate a SHA-256 hash of a file, modify the file, and observe how even a small change produces a completely different hash value.
 
---
 
## Environment
 
- Operating System: Windows  
- Terminal Used: OpenSSL Command Prompt  
- OpenSSL Version: OpenSSL 3.x  
 
---
 
## Steps Performed
 
1. Created a test file using the echo command  
2. Generated a SHA-256 hash of the file using OpenSSL  
3. Modified the file by adding additional text  
4. Generated a new SHA-256 hash after modification  
5. Compared the original and modified hash values  
 
---
 
## Results
 
The original file produced a SHA-256 hash. After modifying the file by adding the word "tampered", a completely different hash was generated.
 
Original Hash:
