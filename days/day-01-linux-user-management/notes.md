# Notes

## What is a Non-Interactive Shell?

A **non-interactive shell** prevents a user from logging into the system via terminal or SSH.

These shells are typically assigned to **service accounts** used by system tools or automation processes.

Common non-interactive shells include:

```
/sbin/nologin
/bin/false
```

---

## Why Use Non-Interactive Users?

In many systems, certain applications require a user account to run processes. However, allowing login access for these accounts would create unnecessary security risks.

Examples of services that use such accounts:

* backup agents
* monitoring tools
* automation services
* system daemons

---

## Security Principle

Using non-interactive users follows the **Principle of Least Privilege**.

This principle means a user or system should only have the permissions necessary to perform its required task and nothing more.

---

## Key Commands Learned

Create a user with custom shell:

```bash
useradd -s /sbin/nologin username
```

Check user details:

```bash
cat /etc/passwd
```

Search for specific user:

```bash
grep username /etc/passwd
```
