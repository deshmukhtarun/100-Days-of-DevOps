# Notes

## What is SELinux?

SELinux (Security-Enhanced Linux) is a Linux security module that provides **mandatory access control (MAC)**.

It restricts how processes interact with files, users, and system resources.

SELinux is commonly used in enterprise Linux distributions such as:

* Red Hat Enterprise Linux
* CentOS
* Rocky Linux
* AlmaLinux

---

## SELinux Modes

SELinux operates in three modes:

| Mode       | Description                                     |
| ---------- | ----------------------------------------------- |
| Enforcing  | SELinux actively blocks unauthorized actions    |
| Permissive | SELinux logs violations but does not block them |
| Disabled   | SELinux is completely turned off                |

---

## Configuration File

SELinux configuration is stored in:

```
/etc/selinux/config
```

Example configuration:

```
SELINUX=disabled
SELINUXTYPE=targeted
```

Changes to this file require a **system reboot** to fully apply.

---

## Why Disable SELinux Temporarily?

During infrastructure changes or application deployments, administrators may temporarily disable SELinux to:

* troubleshoot permission conflicts
* test application behavior
* adjust security policies

After proper configuration, SELinux is usually **re-enabled for security**.

---

## DevOps Context

In production environments, SELinux is often:

* configured with custom policies
* integrated with container platforms
* used to isolate services and applications

Understanding SELinux is important when working with **enterprise Linux systems and secure infrastructure**.
