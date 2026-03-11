# Week 01 Mini Lab — Trust Chain Validation

## Screenshot Evidence

Capture a screenshot of the Certification Path (certificate chain) from your browser.

Save it as:

assets/screenshots/week-01/trust-chain-validation.png

Embed the screenshot below:

![Trust Chain Validation](../../assets/screenshots/week-01/trust-chain-validation.png)

## Website Information

**Website inspected:**  
(https://www.google.com/)
---

## Certificate Chain Breakdown

**Leaf (Server) Certificate**  
*.google.com
**Intermediate Certificate Authority**
WR2

**Root Certificate Authority (Trust Anchor)**
GTS Root R1

---

## Trust Anchor Verification

Is the Root CA marked as trusted by your system?

 Yes 

If yes, explain where that trust comes from (OS/browser root store).
The trust comes from the root certificate store that is already built into the operating system or browser. These stores contain trusted root certificate authorities. When the browser checks a website certificate, it follows the chain until it reaches one of those trusted root certificates.

If no, explain what warning or behavior occurred.

---

## Observations

Document three observations about the certificate.

### Observation 1
I noticed the certificate chain shows different levels, starting with the website certificate and then linking up to the root authority.

### Observation 2
The root certificate authority is at the top of the chain and acts as the trust anchor for the rest of the certificates.
### Observation 3

The browser determines trust by checking each certificate in the chain until it reaches a root certificate authority that is already trusted by the system.

## Reflection

The root certificate is called a trust anchor because it is the starting point of trust for the certificate chain. The browser validates each certificate step by step until it reaches a trusted root certificate authority. If the root certificate was not trusted, the browser would show a security warning and the connection would not be considered secure.

