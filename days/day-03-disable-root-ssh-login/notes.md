# Notes

## Why Disable Root SSH Login?

Allowing direct SSH access to the root account increases security risks because attackers may attempt to guess the root password.

Disabling root login forces administrators to:

1. Log in using a regular user account
2. Use privilege escalation tools such as `sudo`

This improves system accountability and security.

---

## SSH Configuration File

The SSH daemon configuration file is located at:

```
/etc/ssh/sshd_config
```

This file controls how SSH authentication and access behave on the server.

---

## Important SSH Setting

Disable root login by modifying:

```
PermitRootLogin no
```

Possible values include:

* `yes` → root login allowed
* `no` → root login completely disabled
* `prohibit-password` → root login allowed only via SSH keys

---

## Restarting SSH Service

After modifying SSH configuration, the service must be restarted.

```
systemctl restart sshd
```

Without restarting the service, the changes will not take effect.

---

## Security Best Practice

Disabling direct root login is a widely recommended **Linux security best practice** used in production environments.

Benefits include:

* better audit tracking
* reduced brute-force attack surface
* improved privilege management

This practice is common in **DevOps and production infrastructure environments**.
