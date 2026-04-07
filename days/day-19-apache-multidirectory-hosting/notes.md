# Day 19 — Concepts & Notes

## Topic: Apache Multi-Directory Hosting on a Custom Port

---

## Apache Directory Structure (RHEL/CentOS)

```
/etc/httpd/
├── conf/
│   └── httpd.conf          ← main config (Listen port, DocumentRoot, etc.)
├── conf.d/
│   └── *.conf              ← additional virtual host configs
└── conf.modules.d/
    └── *.conf              ← module loading configs

/var/www/
└── html/                   ← default DocumentRoot
    ├── media/              ← served at /media/
    └── games/              ← served at /games/
```

---

## Key Directives in httpd.conf

| Directive | Purpose | Example |
|-----------|---------|---------|
| `Listen` | Port Apache binds to | `Listen 5004` |
| `DocumentRoot` | Root directory for web files | `DocumentRoot "/var/www/html"` |
| `ServerName` | Hostname for the server | `ServerName localhost` |
| `<Directory>` | Per-directory access control | See below |
| `<VirtualHost>` | Virtual host block | `<VirtualHost *:5004>` |

---

## How Sub-path Hosting Works

When files are placed under `DocumentRoot`, Apache serves them at matching URL paths automatically:

```
/var/www/html/media/index.html  →  http://localhost:5004/media/
/var/www/html/games/index.html  →  http://localhost:5004/games/
```

No extra config needed — Apache maps filesystem paths under DocumentRoot directly to URL paths. This is the simplest form of multi-site hosting (path-based, not domain-based).

---

## Virtual Hosting — Types

| Type | How it works | Example |
|------|-------------|---------|
| **Path-based** (this task) | Different dirs under same DocumentRoot | `/media/`, `/games/` |
| **Name-based** | Different `ServerName` per VirtualHost | `media.example.com` vs `games.example.com` |
| **Port-based** | Different ports per VirtualHost | `:5004` vs `:5005` |
| **IP-based** | Different IPs per VirtualHost | `10.0.0.1` vs `10.0.0.2` |

---

## Apache User & File Permissions

Apache runs as the `apache` user (RHEL) or `www-data` (Debian/Ubuntu). Files must be readable by this user:

```bash
# Correct ownership
sudo chown -R apache:apache /var/www/html/

# Correct permissions
# Directories need execute bit (to traverse), files need read bit
sudo chmod -R 755 /var/www/html/      # rwxr-xr-x
```

---

## SELinux & Apache — The Common Gotcha

SELinux maintains a whitelist of ports Apache is allowed to use. Port 80 and 443 are pre-approved; custom ports like 5004 are NOT.

```bash
# Check which ports are allowed for httpd
sudo semanage port -l | grep http_port_t

# Add a new allowed port
sudo semanage port -a -t http_port_t -p tcp 5004

# Fix file context if files were copied from non-standard location
sudo restorecon -Rv /var/www/html/
```

Without this, Apache will refuse to start on port 5004 with a SELinux AVC denial in `/var/log/audit/audit.log`.

---

## scp — Copying Files Between Servers

```bash
# Copy a single file
scp /local/file user@host:/remote/path/

# Copy a directory recursively
scp -r /local/dir user@host:/remote/path/

# Copy from remote to local
scp user@host:/remote/file /local/path/

# Copy between two remote hosts (from jump host)
scp -r /home/thor/media tony@stapp01:/tmp/
```

---

## Apache Config Test & Reload

Always test config syntax before restarting Apache — a syntax error brings down the service:

```bash
sudo apachectl configtest    # test syntax
sudo apachectl graceful      # reload without dropping connections
sudo systemctl restart httpd # full restart (drops connections)
```

---

## Useful Apache Debugging Commands

```bash
# Check what port Apache is listening on
sudo ss -tlnp | grep httpd
sudo netstat -tlnp | grep httpd

# View real-time access log
sudo tail -f /var/log/httpd/access_log

# View real-time error log
sudo tail -f /var/log/httpd/error_log

# Check SELinux denials
sudo ausearch -c httpd --raw | audit2why

# Check Apache process
ps aux | grep httpd
```

---

## Troubleshooting Decision Tree

```
curl http://localhost:5004/media/ fails
│
├─ Connection refused?
│   └─ Apache not running → systemctl status httpd → check error_log
│
├─ Forbidden (403)?
│   └─ Permissions issue → chown/chmod, or SELinux context (restorecon)
│
├─ Not Found (404)?
│   └─ Files not in the right path → ls /var/www/html/
│
└─ Apache won't start?
    ├─ Syntax error → apachectl configtest
    └─ Port blocked → semanage port -a -t http_port_t -p tcp 5004
```

---