# Lab — Encryption & Decryption

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

Original Hash:

C:\Users\kim>type labs\02-week-02-cryptography-fundamentals\submissions\hashes\message.sha256.txt
SHA2-256(labs\02-week-02-cryptography-fundamentals\submissions\hashes\message.txt)= 0a2ae40887d2eeb805395e8744e7657b0a2fe2c7d68ae928bf883ebf92dc57ae

Modified Hash:

C:\Users\kim>type labs\02-week-02-cryptography-fundamentals\submissions\hashes\message_tampered.sha256.txt
SHA2-256(labs\02-week-02-cryptography-fundamentals\submissions\hashes\message.txt)= 17fd1d653bf6a736d399c9f80f78b05b79aed7f7fa90223b1bd228e08b9bf4ea

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
