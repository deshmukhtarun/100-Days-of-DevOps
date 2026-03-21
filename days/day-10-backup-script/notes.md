# Notes

## Backup Workflow

This task demonstrates a basic backup pipeline:

1. Compress website files
2. Store locally
3. Transfer to remote storage

---

## Zip Command

```
zip -r archive.zip directory
```

* `-r` → recursive (includes all files)

---

## SCP Command

```
scp file user@host:/path
```

Used for secure file transfer over SSH.

---

## Password-less SSH

```
ssh-keygen
ssh-copy-id user@host
```

Enables automation without password prompts.

---

## Script Permissions

```
chmod 755 script.sh
```

Ensures:

* executable by all users
* safe permission model

---

## Important Constraints

* No `sudo` inside script
* Correct archive name required
* Correct directory path required
* Correct remote user and host required

---

## Common Failure Reasons

* Wrong script name
* Wrong source directory
* Wrong zip file name
* SSH not configured properly
* Script not executable

---

## DevOps Context

This is a simplified version of real-world backups:

* application backups
* database dumps
* log archiving
* disaster recovery

Later evolved into:

* cron jobs
* cloud storage (S3)
* backup pipelines
