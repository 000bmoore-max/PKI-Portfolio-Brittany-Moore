# Part A — Explore Three Built-in Templates

## Template 1 — User

### General
- Display Name: User
- Internal Name: User
- Validity Period: 1 year
- Schema Version: Version 1

### Request Handling
- Purpose: Signature and Encryption

### Subject Name
- Subject name is built from Active Directory information

### Extensions — Key Usage
- Digital Signature
- Key Encipherment
- Data Encipherment

### Extensions — EKU / Application Policies
- Client Authentication
- Secure Email
- Encrypting File System

---

## Template 2 — Computer

### General
- Display Name: Computer
- Internal Name: Computer
- Validity Period: 1 year

### Request Handling
- Purpose: Signature and Encryption

### Subject Name
- Subject name is built from Active Directory information

### Extensions — Key Usage
- Digital Signature
- Key Encipherment

### Extensions — EKU / Application Policies
- Client Authentication
- Server Authentication

---

## Template 3 — Web Server

### General
- Display Name: Web Server
- Validity Period: 2 years

### Request Handling
- Purpose: Encryption and Server Authentication

### Subject Name
- Subject name is supplied in the request

### Extensions — Key Usage
- Digital Signature
- Key Encipherment

### Extensions — EKU / Application Policies
- Server Authentication

---

# Template Comparison Questions

## What is the most significant difference between the User, Computer, and Web Server templates?

The User and Computer templates automatically build subject information from Active Directory accounts and computer objects. The Web Server template differs because it allows the requester to manually supply the subject name. This is important because web servers often require custom DNS names that may not directly match Active Directory computer objects. The templates also differ in purpose, with the Web Server template being specifically designed for TLS and HTTPS server authentication.

---

## Why does the Web Server template use “Supply in the request” for the subject name?

The Web Server template uses “Supply in the request” because web servers often host multiple websites and DNS names that are not automatically stored in Active Directory. Allowing the requester to manually specify the subject name makes the template flexible for public websites, load balancers, reverse proxies, and enterprise web applications. This allows administrators to request certificates containing specific hostnames and Subject Alternative Names (SANs) required for HTTPS communication.

---

# Reflection

## Why does AD CS require you to duplicate a built-in template rather than modifying it directly?

AD CS protects built-in templates to prevent administrators from accidentally breaking default enterprise certificate configurations. Duplicating templates allows organizations to customize settings safely while preserving Microsoft’s original secure baseline templates. This also supports change management and environment-specific certificate policies.

---

## One setting in the CVI-WebServer template that you found unexpected or want to explore further.

One setting that was interesting was the “Supply in the request” subject name option. It demonstrated how administrators can manually define DNS names and certificate identities rather than relying only on Active Directory information. This setting appears especially important for enterprise web applications and public-facing HTTPS services.
