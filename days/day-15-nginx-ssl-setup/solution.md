# Solution: Day 15 - Nginx SSL Configuration

### Step 1: Install Nginx
Log into App Server 3 (stapp03) and install the Nginx package:
- Command: ssh banner@stapp03
- Command: sudo yum install -y nginx

### Step 2: Secure the SSL Certificates
Move the provided certificate and key from the temporary directory to a standard Nginx SSL directory:
- Command: sudo mkdir -p /etc/nginx/ssl
- Command: sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
- Command: sudo mv /tmp/nautilus.key /etc/nginx/ssl/

### Step 3: Configure Nginx for HTTPS
Edit the Nginx configuration file (/etc/nginx/nginx.conf) and ensure a server block is configured for port 443 with the following settings:
- listen 443 ssl http2;
- server_name stapp03;
- root /usr/share/nginx/html;
- ssl_certificate "/etc/nginx/ssl/nautilus.crt";
- ssl_certificate_key "/etc/nginx/ssl/nautilus.key";

### Step 4: Create Web Content
Create the required index file in the Nginx document root:
- Command: echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html

### Step 5: Start and Enable Service
Verify the configuration syntax and restart the service:
- Command: sudo nginx -t
- Command: sudo systemctl start nginx
- Command: sudo systemctl enable nginx

### Step 6: Final Verification
From the jump-host, test the HTTPS connection:
- Command: curl -Ik https://stapp03/
- Result: 200 OK response with SSL certificate details.