# Day 16 — Concepts & Notes

## Topic: Nginx as a Reverse Proxy / Load Balancer

---

## What is a Load Balancer?

A load balancer distributes incoming network traffic across multiple backend servers (called an **upstream group**). This improves:
- **Availability** — if one server fails, traffic goes to others
- **Scalability** — multiple servers share the load
- **Performance** — no single server gets overwhelmed

---

## Nginx Load Balancing — Key Directives

### `upstream` block
Defines a group of backend servers.

```nginx
upstream my_group {
    server backend1:8080;
    server backend2:8080;
    server backend3:8080;
}
```

### `proxy_pass`
Forwards requests from Nginx to the upstream group.

```nginx
location / {
    proxy_pass http://my_group;
}
```

### `listen`
Tells Nginx which port to accept incoming connections on.

```nginx
server {
    listen 80;
}
```

---

## Load Balancing Algorithms

| Algorithm | Directive | Behaviour |
|-----------|-----------|-----------|
| Round Robin | *(default, no directive needed)* | Requests go to each server in turn |
| Least Connections | `least_conn;` | Sends to server with fewest active connections |
| IP Hash | `ip_hash;` | Same client IP always goes to same server (session persistence) |
| Weighted | `weight=N` on each server | Proportional traffic distribution |

Example with weights:
```nginx
upstream my_group {
    server stapp01:8080 weight=3;
    server stapp02:8080 weight=1;
    server stapp03:8080 weight=1;
}
```

---

## Important: Config File Hierarchy in Nginx

```
/etc/nginx/
├── nginx.conf          ← main config (edited in this task)
├── conf.d/             ← additional server blocks (included by nginx.conf)
└── sites-enabled/      ← Debian/Ubuntu style (symlinked from sites-available)
```

In this task, we edited **only** `/etc/nginx/nginx.conf` as required.

---

## Common Nginx Commands

```bash
nginx -t                        # Test config syntax
systemctl start nginx           # Start nginx
systemctl restart nginx         # Restart nginx
systemctl reload nginx          # Reload config without dropping connections
systemctl enable nginx          # Enable on boot
systemctl status nginx          # Check status
nginx -s reload                 # Alternative reload
```

---

## Apache vs Nginx — Quick Comparison

| Feature | Apache (httpd) | Nginx |
|---------|---------------|-------|
| Architecture | Process/thread per request | Event-driven, async |
| Config style | `.htaccess` per-directory | Centralized config |
| Load balancing | Via `mod_proxy_balancer` | Built-in, simpler |
| Static files | Good | Excellent |
| Use case here | App servers (backend) | LBR (frontend proxy) |

---

## Key Lesson from This Task

- The **LBR server (stlb01)** listens on port **80** facing the internet/client
- The **App servers (stapp01-03)** run Apache on port **8080** (internal)
- Nginx proxies requests from port 80 → upstream group → Apache on 8080
- **Never change the backend port** in the app server config — only reference it in Nginx's upstream block

---

## Troubleshooting Tips

| Problem | Check |
|---------|-------|
| `nginx -t` fails | Syntax error in nginx.conf — re-check brackets and semicolons |
| `curl` returns connection refused | Nginx not running or wrong port |
| `curl` returns 502 Bad Gateway | App server unreachable or Apache down |
| `curl` returns 504 Gateway Timeout | App server too slow or firewall blocking |

```bash
# Debug: check if app servers are reachable from LBR
curl http://stapp01:8080
curl http://stapp02:8080
curl http://stapp03:8080
```
