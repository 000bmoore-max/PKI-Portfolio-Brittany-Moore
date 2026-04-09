
---

### The Five-Step Workflow (Always in Order)

#### 1. Generate a Private Key
- Created on the requester’s device
- Produces a key pair (private + public)
- Private key is never shared or transmitted

#### 2. Create a CSR (Certificate Signing Request)
- Contains identity information:
  - Common Name (CN)
  - Organization (O)
  - Organizational Unit (OU)
  - Country
- Includes the public key
- Output file: `.csr`

#### 3. Submit to the CA
- CSR is sent to the CA or Registration Authority (RA)
- CA performs identity verification (out-of-band)
- Private key is NOT included

#### 4. CA Validates and Signs
- CA verifies identity
- Combines identity + public key
- Signs using CA’s private key
- Output: signed certificate (`.crt`)

#### 5. Certificate Deployment
- Certificate is returned to requester
- Installed on systems (servers, apps, devices)
- Systems that trust the CA will trust the certificate

---

### Core Security Rule
The private key NEVER leaves the requester’s device.  
If it is exposed, the identity can be compromised.

---

### Certificate vs CSR vs Key
- Private Key → Secret, stays local
- Public Key → Shared through CSR
- CSR → Identity request + public key
- Certificate → Signed and trusted identity

---

### Trust Model
Trust is inherited from the CA.  
If a system trusts the CA, it will trust all certificates signed by that CA.

---

### Why This Matters
- Foundation of PKI
- Used in:
  - HTTPS / TLS
  - VPN authentication
  - Smartcards
  - Code signing
  - Enterprise identity systems

---

### Key Insight
The CA issues the certificate — NOT the private key.  
The private key always remains under the requester’s control.
