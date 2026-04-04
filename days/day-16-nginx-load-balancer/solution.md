# Day 16 — Solution

## Step 1: Verify Apache on All App Servers

SSH into each app server, check Apache status, and note its listening port.

```bash
# App Server 1
ssh tony@stapp01
sudo systemctl status httpd
sudo grep -i "^Listen" /etc/httpd/conf/httpd.conf
# If not running:
sudo systemctl start httpd && sudo systemctl enable httpd
exit
```

```bash
# App Server 2
ssh steve@stapp02
sudo systemctl status httpd
sudo grep -i "^Listen" /etc/httpd/conf/httpd.conf
exit
```

```bash
# App Server 3
ssh banner@stapp03
sudo systemctl status httpd
sudo grep -i "^Listen" /etc/httpd/conf/httpd.conf
exit
```

> Apache typically listens on port **8080** in this environment. Do NOT change it.

---

## Step 2: SSH into Load Balancer

```bash
ssh loki@stlb01
```

---

## Step 3: Install Nginx (if not already installed)

```bash
sudo yum install -y nginx
```

---

## Step 4: Configure Nginx as Load Balancer

Edit ONLY the main Nginx config file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Replace the contents with:

```nginx
events {}

http {
    upstream nautilus_app {
        server stapp01:8080;
        server stapp02:8080;
        server stapp03:8080;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://nautilus_app;
        }
    }
}
```

> **Note:** Replace `8080` with the actual Apache port found in Step 1 if it differs.

---

## Step 5: Validate and Restart Nginx

```bash
# Test config syntax
sudo nginx -t

# Restart and enable nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

## Step 6: Verify Load Balancer is Working

```bash
curl http://stlb01:80
```

Expected: HTML response from one of the app servers.
Run multiple times to see round-robin distribution across servers.
