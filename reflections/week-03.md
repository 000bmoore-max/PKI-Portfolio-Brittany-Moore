# Week 3 Reflection

## What did you learn this week?
This week focused on understanding X.509 certificates  
and how they are structured and used in PKI.

I learned how to break down a certificate into parts  
like the subject, issuer, validity period, public key,  
and signature.

We also focused on extensions like Subject Alternative Name (SAN),  
Key Usage, and Extended Key Usage (EKU), and how they define  
what a certificate is allowed to do.

I also learned how certificate chains work and how trust  
is built from the root certificate down to the server.

---

## What concept was most challenging?
The most challenging part was understanding how all the  
certificate extensions work together.

At first, SAN, Key Usage, and EKU seemed similar,  
but I realized they each control something different  
like identity, permissions, and purpose.

It also took a minute to fully understand how trust  
is passed through the certificate chain.

---

## Where does this concept appear in real-world systems?
This appears anytime you visit a secure website using HTTPS.

The browser checks the certificate fields and extensions  
to make sure the site is valid and trusted.

Organizations also use certificates for authentication,  
secure communication, and controlling access to systems.

If anything is misconfigured, users will see security  
warnings or blocked connections.

---

## How would you explain this topic to a non-technical audience?
A certificate is like a digital ID card for a website.

It shows who owns the site, who verified it, and what  
it is allowed to do.

There are also rules attached to it that tell systems  
how to trust it, and if anything is wrong, the system  
will not accept it.

---

## What questions remain?
I want to understand more about how certificate authorities  
decide what permissions to include in certificates.

I also want to learn more about how misconfigurations  
are detected and fixed in real environments.
