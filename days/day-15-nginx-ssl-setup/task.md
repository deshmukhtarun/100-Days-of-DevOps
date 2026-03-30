# Task: Day 15 - Deploy Nginx with SSL on App Server 3

## Scenario
The system admins team needs to prepare App Server 3 (stapp03) in the Stratos Datacenter for a new application deployment. The server requires Nginx installation and configuration with a provided self-signed SSL certificate.

## Requirements
1. Install and configure Nginx on App Server 3.
2. Move the self-signed SSL certificate (`nautilus.crt`) and key (`nautilus.key`) from `/tmp` to an appropriate secure location.
3. Deploy the SSL certificate and key in the Nginx configuration.
4. Create an `index.html` file with the content "Welcome!" under the Nginx document root.
5. Validation: Access the server via HTTPS from the jump host using "curl -Ik https://stapp03/".