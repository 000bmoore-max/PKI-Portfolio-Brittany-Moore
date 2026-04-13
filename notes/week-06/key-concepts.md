# Week 6 — PKI Diagnostics Key Takeaways

## Core Concept
PKI troubleshooting follows a structured process:
1. Retrieve the certificate
2. Parse certificate details
3. Validate the certificate chain
4. Check revocation and trust

Each TLS issue can be traced by isolating which step fails.

---

## Lab 01 — Expired Certificate
- TLS failed because the certificate was past its **NotAfter date**
- Even with a valid chain, expired certificates are automatically untrusted
- Key takeaway: **Validity period is a hard requirement**

---

## Lab 02 — Broken Chain
- TLS failed due to a **missing intermediate certificate**
- Server did not provide full chain → client could not build trust path
- Key takeaway: **Servers must present full certificate chain**

---

## Lab 03 — SAN / Hostname Mismatch
- TLS failed because hostname did NOT match certificate SAN
- Wildcard (*.domain.com) only covers one level of subdomain
- wrong.host.badssl.com ≠ *.badssl.com
- Key takeaway: **Hostname validation is separate from chain validation**

---

## Important Patterns Learned
- TLS errors can look similar but have different root causes:
  - Expired → time issue
  - Broken chain → missing trust path
  - SAN mismatch → identity mismatch

- OpenSSL is used to:
  - Retrieve certs (`s_client`)
  - Inspect certs (`x509 -text`)
  - Validate trust (`verify`)

---

## Real-World Understanding
- You are NOT fixing infrastructure yet
- You are learning to:
  - Diagnose issues
  - Identify root cause
  - Communicate fixes clearly

This is exactly what SOC analysts and security engineers do first.

---

## Key Skill Built
- Reading TLS errors
- Understanding certificate structure (CN, SAN, Issuer)
- Differentiating between:
  - Certificate problems
  - Chain problems
  - Configuration problems

---

## Big Picture
PKI failures are not random — they fall into patterns:
- Expiration
- Trust chain issues
- Identity mismatch

Once you identify the category, the fix becomes straightforward.
