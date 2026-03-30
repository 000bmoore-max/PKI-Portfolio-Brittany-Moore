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

