# Lab 1 — Encryption & Decryption

## Overview
This lab focused on understanding how symmetric encryption works using OpenSSL. The goal was to encrypt a file, decrypt it, and verify that the original data remained unchanged.

---

## Environment
- Operating System: Windows
- Terminal Used: OpenSSL Command Prompt
- OpenSSL Version: OpenSSL 3.x

---

## Steps Performed
1. Created a plaintext file containing test data.
2. Used OpenSSL to encrypt the file using a symmetric algorithm.
3. Decrypted the encrypted file back into plaintext.
4. Compared the original and decrypted files to confirm they matched.

---

## Results
- Successfully generated an encrypted file.
- Successfully decrypted the file.
- The decrypted file matched the original plaintext file.

---

## Key Findings
• Encryption protects data by converting it into unreadable format  
• Decryption restores the original data using the correct key  
• Matching files confirm integrity after decryption  

---

## Explanation
Encryption ensures confidentiality by protecting data from unauthorized access. Decryption is required to restore the original data. If the decrypted file matches the original, it confirms that the encryption and decryption process worked correctly without data loss.

---

## Challenges / Troubleshooting
Some commands initially failed due to incorrect syntax, but were resolved by correcting command structure and file paths.

---

## Artifacts
- plaintext.txt  
- plaintext.txt.enc  
- plaintext.decrypted.txt  
