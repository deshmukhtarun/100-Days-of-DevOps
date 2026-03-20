# Notes

## MariaDB Service Failure – Root Cause

The MariaDB service failed to start because the **data directory was missing**:

```
/var/lib/mysql
```

Without this directory, the database cannot initialize or run.

---

## Key Observation

```
journalctl -xeu mariadb
```

returned:

```
-- No entries --
```

This is a critical clue.

### Meaning:

* Service did not reach runtime stage
* Logging never started
* Likely causes:

  * missing data directory
  * broken configuration
  * permission issues

---

## Database Initialization

MariaDB requires initial system tables to function.

Command used:

```
mariadb-install-db --user=mysql --datadir=/var/lib/mysql
```

This creates:

* system tables
* default database structure
* required metadata files

---

## Importance of Permissions

```
chown -R mysql:mysql /var/lib/mysql
```

Ensures:

* database process can read/write data
* prevents startup failures due to permission issues

---

## Troubleshooting Approach

This task followed a standard DevOps debugging workflow:

1. Identify issue (service down)
2. Check service status
3. Check logs
4. Observe abnormal behavior (no logs)
5. Inspect filesystem
6. Identify missing components
7. Fix root cause
8. Restart service
9. Verify

---

## Real DevOps Insight

This is a common real-world issue when:

* new servers are provisioned
* storage volumes are not mounted
* database setup is incomplete
* data directories are deleted or corrupted

---

## Key Learning

```
No logs ≠ no issue
```

It often means:

```
Service failed before logging started
```

This requires checking:

* filesystem
* configuration
* dependencies
