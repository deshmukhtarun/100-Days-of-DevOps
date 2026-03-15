# Notes

## Linux File Permissions

Linux uses three types of permissions:

* **Read (r)** → view file contents
* **Write (w)** → modify the file
* **Execute (x)** → run the file as a program or script

Permissions are assigned to three categories:

* owner
* group
* others

---

## chmod Command

The `chmod` command is used to change file permissions.

Example:

```bash
chmod +x filename
```

This adds execute permission.

Example from this task:

```bash
chmod +x /tmp/xfusioncorp.sh
```

---

## Permission Representation

Example permission string:

```
-rwxr-xr-x
```

Meaning:

| Section | Meaning            |
| ------- | ------------------ |
| rwx     | owner permissions  |
| r-x     | group permissions  |
| r-x     | others permissions |

---

Example used in this task:

```bash
chmod 755 /tmp/xfusioncorp.sh
```

Permission breakdown:

| Value | Permission |
|------|------|
| 7 | read + write + execute |
| 5 | read + execute |
| 5 | read + execute |

This results in:

```
rwxr-xr-x
```

Which allows all users to execute the script while restricting write access to the owner.

---

## Why Execute Permission Matters

Scripts must have execute permission in order to run.

Without execute permission, attempting to run a script will produce an error such as:

```
Permission denied
```

Granting execute permission allows the script to be executed by users or automation systems.

---

## Real DevOps Context

Automation scripts like backup scripts, deployment scripts, or maintenance scripts must have correct execution permissions so that:

* system administrators can run them
* cron jobs can execute them
* automation tools can trigger them

Proper permission management ensures both **functionality and security** in production systems.
