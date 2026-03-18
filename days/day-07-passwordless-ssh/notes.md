# Notes

## What is Password-less SSH?

Password-less SSH allows a user to log into a remote server **without entering a password**.

It uses **public-key cryptography** instead of passwords.

---

## How It Works

1. A key pair is generated:

   * private key (kept secure)
   * public key (shared with servers)

2. The public key is added to:

```
~/.ssh/authorized_keys
```

on the remote server.

3. When connecting, SSH verifies the private key automatically.

---

## Key Commands

Generate key:

```
ssh-keygen
```

Copy key:

```
ssh-copy-id user@host
```

Connect:

```
ssh user@host
```

---

## Why This Is Important

Password-less SSH is essential for:

* automation scripts
* configuration management tools (Ansible)
* CI/CD pipelines
* remote server orchestration

Without it, automation would require manual password input.

---

## Security Benefits

* eliminates password-based login risks
* reduces brute-force attack surface
* enables controlled access using keys

---

## DevOps Context

This is a foundational concept used in:

* Ansible (agentless automation)
* Jenkins pipelines
* deployment scripts
* server provisioning

Almost all DevOps workflows rely on SSH key-based authentication.