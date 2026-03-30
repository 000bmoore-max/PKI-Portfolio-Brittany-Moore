## Key Concepts

- Certificate vs Format:
A certificate contains identity data (subject, issuer, validity, public key),
while the format determines how that data is packaged and stored.

- PEM Format:
Base64 encoded and readable in a text editor.
Uses BEGIN/END markers.
Common in Linux systems and web servers.

- DER Format:
Binary version of the same certificate data.
Not readable in a text editor.
Common in Java applications and older enterprise systems.

- PFX / PKCS#12:
Container format that includes certificate, private key, and chain.
Password protected and used for secure transfer between systems.
Important because it contains private key material.

- File Extensions Are Misleading:
Extensions like .cer, .crt, and .pem do not reliably indicate encoding.
Files must be inspected to determine actual format.

- Format Conversion:
Converting between PEM and DER does not change certificate data.
Only the encoding changes, so no new signature is required.

- Private Key Protection:
Private keys should never be exposed or stored unprotected.
PFX files require passwords because they contain sensitive key material.

- Trust Store:
A certificate is only trusted if its root CA exists in the system’s trust store.
Trust is not automatic — it is enforced by the system.

- Chain of Trust:
Validation works by building a chain from leaf → intermediate → root.
The root CA must already be trusted for validation to succeed.

- Trust Decision:
Trust is a policy decision controlled by the OS or organization.
Adding a root CA grants it authority and must be carefully managed.

- Common Trust Store Mistakes:
Installing certificates in the wrong store (user vs machine)
Assuming all browsers use the same trust store
Failing to update trust stores (especially on Linux)
📌 Reflection (still short, still you)
## Reflection

This week helped me understand that certificates don’t change, but the way they are stored
and transferred does. I learned that formats like PEM, DER, and PFX all contain the same
core data, but they behave differently depending on the system.

One thing that stood out is that file extensions can’t be trusted to tell you the format,
so you have to actually inspect the file. That changed how I look at certificates.

I also understand now that trust is not automatic.
A certificate only works if the root CA is already in the trust store,
which explains why some certificates fail even when they look valid.

This week made the process feel more real, especially seeing how small mistakes
with format or trust stores can cause issues in real environments.
✅ Where this goes
notes/week-04/key-concepts.md → paste Key Concepts
reflections/week-04.md → paste Reflection

This is:

based on what YOU wrote/screenshotted
cleaned up (so it looks professional)
still sounds like you
strong enough for portfolio review later

If you want next:
👉 I can turn this into interview answers (this is actually good PKI talk now)
👉 Or we jump straight into Lab 03 and finish this whole week clean in one go

---

## Challenges / Troubleshooting

At first, understanding the difference between formats  
was confusing because they represent the same certificate  
in different ways.

It also took some time to understand why the DER file  
looked unreadable and how conversions worked without  
changing the actual data.

---

## Artifacts

- leaf_cert.pem  
- leaf_cert.der  
- leaf_cert_restored.pem  
- test_cert.pem  
- test_bundle.pfx 
