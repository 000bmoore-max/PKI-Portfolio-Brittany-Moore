# Lab 01 — Certificate Formats (Week 4)

## Results

- **What did the PEM file look like compared to the DER file?**  
The PEM file was readable and had text with headers like  
"-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----".  
The DER file looked like random binary data and was not readable.

- **What happened when you opened the .der file in a text editor?**  
It showed unreadable characters and symbols because DER  
is a binary format, not meant for human reading.

- **What did the diff output show after converting PEM → DER → PEM?**  
The diff showed no meaningful differences.  
This confirmed that the conversion process did not change  
the certificate data, only the format.

- **What information was displayed when you verified the PFX?**  
It showed the certificate details along with the private key.  
It also confirmed the certificate chain and that the bundle  
contained both the certificate and its associated key.

---

## Key Findings

PEM files are human-readable while DER files are binary.  
Converting between formats does not change the certificate  
data itself, only how it is stored.

PFX files are different because they bundle certificates  
and private keys together and require a password for protection.

---

## Explanation

- **Why does a PFX require a password?**  
A PFX file contains a private key, which must be protected.  
The password encrypts the file so unauthorized users cannot  
access the private key.

- **In what real-world scenario would you choose PEM vs DER vs PFX?**  
PEM is used when working with servers like Apache or Nginx  
because it is readable and easy to manage.

DER is used in systems that require a binary format,  
like some Windows environments or APIs.

PFX is used when you need to securely transfer both  
a certificate and its private key together, such as  
importing into a browser or Windows certificate store.

- **Why is it important never to commit private key files to GitHub?**  
Private keys must remain secret. If exposed, anyone can  
impersonate the system or decrypt sensitive data.  
This can lead to major security breaches and loss of trust.

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
