# Day 19 — Solution

## Step 1: Copy Website Directories from Jump Host to App Server 1

From the jump host, use `scp` to transfer both website folders to `stapp01`:

```bash
# On jump_host (logged in as thor)
scp -r /home/thor/media tony@stapp01:/tmp/
scp -r /home/thor/games tony@stapp01:/tmp/
```

---

## Step 2: SSH into App Server 1

```bash
ssh tony@stapp01
```

---

## Step 3: Install Apache (httpd)

```bash
sudo yum install -y httpd
```

---

## Step 4: Move Website Content to Apache Document Root

```bash
sudo mv /tmp/media /var/www/html/
sudo mv /tmp/games /var/www/html/
```

Verify:

```bash
ls /var/www/html/
# Expected: media  games
```

> **Note:** If `mv` throws `inter-device move failed: unable to remove target: Directory not empty`, the directories are already in place from a prior attempt — this is fine, just continue.

---

## Step 5: Configure Apache to Listen on Port 5004

Edit the main Apache config file:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Find and change the `Listen` directive:

```apache
# Change this:
Listen 80

# To this:
Listen 5004
```

Also update the `VirtualHost` block if present:

```apache
# Change:
<VirtualHost *:80>

# To:
<VirtualHost *:5004>
```

---

## Step 6: Set Correct Ownership & Permissions

```bash
sudo chown -R apache:apache /var/www/html/media
sudo chown -R apache:apache /var/www/html/games
sudo chmod -R 755 /var/www/html/media
sudo chmod -R 755 /var/www/html/games
```

---

## Step 7: Firewall & SELinux

### Firewall — allow port 5004

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --add-port=5004/tcp
sudo firewall-cmd --reload
```

### SELinux — check status first

```bash
sestatus
```

- If `disabled` or `permissive` → **skip SELinux steps entirely**, proceed to Step 8.
- If `enforcing` → install the utils and add the port:

```bash
# RHEL 9 package name (not policycoreutils-python)
sudo yum install -y policycoreutils-python-utils

sudo semanage port -a -t http_port_t -p tcp 5004
sudo restorecon -Rv /var/www/html/
```

> **Real-world note (RHEL 9):** If `semanage` returns `ValueError: SELinux policy is not managed or store cannot be accessed`, SELinux is disabled on the host — skip it and move on.

---

## Step 8: Start and Enable Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

---

## Step 9: Verify with curl

```bash
curl http://localhost:5004/media/
curl http://localhost:5004/games/
```

Both should return the HTML content of the respective websites.

---

## Troubleshooting

Always test config syntax before starting Apache:

```bash
sudo apachectl configtest
# or
sudo httpd -t
```

```bash
# Check what port Apache ended up listening on
sudo ss -tlnp | grep httpd

# View real-time error log
sudo tail -f /var/log/httpd/error_log
```

| Symptom | Cause | Fix |
|---------|-------|-----|
| `mv` fails with "Directory not empty" | Files already exist from prior attempt | Safe to ignore — files are in place |
| Apache won't start | Syntax error in httpd.conf | Run `apachectl configtest` |
| `semanage` not found | Package missing on RHEL 9 | `yum install -y policycoreutils-python-utils` |
| `semanage` ValueError | SELinux is disabled on host | Skip SELinux steps entirely |
| curl: Connection refused | Apache not running or wrong port | `systemctl status httpd`, check `grep ^Listen httpd.conf` |
| curl: 403 Forbidden | File permission issue | Re-run `chown` and `chmod` from Step 6 |