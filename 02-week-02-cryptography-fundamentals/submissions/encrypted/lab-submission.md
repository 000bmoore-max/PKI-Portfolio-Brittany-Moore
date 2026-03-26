# Lab — -Week2 

## Overview
This lab focused on understanding how symmetric encryption works using OpenSSL. The goal was to encrypt a plaintext file, decrypt it, and verify that the original data remained the same after the process.

---

## Environment

- Operating System: Windows  
- Terminal Used: OpenSSL Command Prompt  
- OpenSSL Version: OpenSSL32

---

## Steps Performed

1. Created a plaintext file using the echo command  
2. Used OpenSSL to encrypt the file with AES-256-CBC and a password  
3. Decrypted the encrypted file using the same password  
4. Verified both files matched using the fc command  

---

## Results

The plaintext file was successfully encrypted and became unreadable (ciphertext). After decryption, the file returned to its original readable form.

Verification was done using the `fc` command, which showed:

FC: no differences encountered

This confirmed that the encryption and decryption process worked correctly.
![Verification](.../../../assets/screenshots/week-02/verification.png
---

## Key Findings

- The encrypted file appears as unreadable data (ciphertext)  
- Decryption only works with the correct password  
- The original file and decrypted file matched exactly  
- Symmetric encryption protects data confidentiality  

---

## Explanation

These results show that symmetric encryption protects data by converting it into unreadable ciphertext. Only someone with the correct password can decrypt the data back to its original form. This is important in real-world systems like TLS because it ensures that sensitive information cannot be read by unauthorized users.

---

## Challenges / Troubleshooting

One issue I encountered was that some commands like `openssl` and `diff` were not recognized in PowerShell. I resolved this by using the OpenSSL command prompt and using the `fc` command instead of `diff` to compare files.

---

## Artifacts

- plaintext.txt  
- plaintext.txt.enc  
- plaintext.decrypted.txt  
- verification screenshot stored in assets/screenshots/week-02/
