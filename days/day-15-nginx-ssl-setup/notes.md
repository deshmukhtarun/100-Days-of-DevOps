# DevOps Concept Notes: Nginx SSL Termination

### 1. SSL/TLS Handshake
Nginx acts as the "terminator" for SSL/TLS traffic. It handles the decryption of incoming HTTPS requests on port 443 and serves the encrypted content back to the client using the private key and public certificate.

### 2. Standard SSL Locations
While certificates can technically sit anywhere, Linux standards (FHS) suggest placing certificates in /etc/pki/tls/certs or /etc/nginx/ssl and private keys in /etc/pki/tls/private or /etc/nginx/ssl. It is critical to ensure 600 permissions for private keys to prevent unauthorized access.

### 3. Curl with Insecure Flags
When testing self-signed certificates, the "curl -k" (or --insecure) flag is mandatory. This tells curl to proceed even though the certificate wasn't issued by a trusted Certificate Authority (CA) like Let's Encrypt or DigiCert.

### 4. Nginx vs. Apache Syntax
Unlike Apache's "SSLCertificateFile" directive, Nginx uses "ssl_certificate". Additionally, Nginx configuration changes are best validated using "nginx -t" before restarting to prevent service outages due to syntax errors.