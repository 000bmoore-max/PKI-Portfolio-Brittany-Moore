# Lab 01 — Convert Certificate Formats

## Environment
Windows environment using OpenSSL (Win64)

Used Command Prompt and Git Bash

Worked in C:\Users\kim directory

---

## Steps

Retrieved a certificate and saved it as leaf_cert.pem

Converted PEM → DER using OpenSSL

Converted DER → PEM to restore certificate

Compared original and restored files

Generated a self-signed certificate and private key

Created a PFX bundle using certificate + private key

---

## Results

PEM file was readable text format

DER file was binary and not readable in text editor

Converted PEM → DER → PEM successfully

Certificate content remained unchanged

PFX file was created and bundled successfully

---

## Key Findings

Certificate formats change encoding, not the certificate itself

PEM is readable and commonly used

DER is binary and system-focused

PFX bundles certificate + private key and requires a password

---

## Explanation

PFX requires a password because it contains a private key

PEM format is used for readability and configuration

DER format is used for compact binary storage

PFX is used in Windows environments for secure certificate transport

Private keys should never be uploaded or exposed

---

## Challenges / Troubleshooting

File naming issues such as .pem.pem caused command failures

Incorrect file paths caused "file not found" errors

Git Bash path handling created confusion

Command syntax errors like missing "-" flags caused failures

Repeated prompts during certificate creation slowed progress

Switching to Command Prompt resolved execution issues

Understanding when password prompts occur fixed confusion

---

## Artifacts

leaf_cert.pem

leaf_cert.der

leaf_cert_restored.pem

test_cert.pem

test_bundle.pfx
