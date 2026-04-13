## 🛠️ Tools Used
- OpenSSL
- PowerShell
- Certificate files (.pem)

## ⚙️ Steps Performed
I navigated to the correct directory and verified my certificate files were in `.pem` format. I then ran the OpenSSL OCSP command using the issuer and leaf certificate. The output was redirected to a text file for review. I analyzed the response to confirm the certificate status.

## 📊 Results
The OCSP response showed the certificate status as **good**, meaning it has not been revoked. The response included timestamps and certificate details. This confirms the certificate is valid and trusted.

## 🚧 Challenges
I ran into issues with incorrect file extensions and PowerShell not recognizing OpenSSL. I resolved this by correcting file types and using the full OpenSSL path. I also had minor Git issues while pushing changes.

## 🧠 Takeaways
This lab helped me understand how OCSP works in verifying certificate status. I gained hands-on experience using OpenSSL and troubleshooting command-line errors. This is an important skill for working with PKI systems.

## 📂 Artifacts
- google_output.txt  
- issuer_cert_w5.pem  
- leaf_cert_w5.pem  
- ocsp_response.txt
