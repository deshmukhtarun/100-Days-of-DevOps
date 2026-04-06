# Day 18 — Concepts & Notes

## Topic: MariaDB Server Installation & Database Management

---

## MariaDB vs MySQL

MariaDB is a community-maintained fork of MySQL, created when Oracle acquired MySQL. They are largely compatible — the same SQL syntax, same client tools, and same `mysql` command works for both.

| Feature | MariaDB | MySQL |
|---------|---------|-------|
| License | GPL (fully open source) | Dual (GPL + commercial) |
| Default in RHEL/CentOS 7+ | ✅ Yes | ❌ No |
| Syntax compatibility | Near 100% with MySQL | — |
| Storage engines | InnoDB, Aria, MyISAM | InnoDB, MyISAM |
| Command to connect | `mysql` | `mysql` |

---

## MariaDB vs PostgreSQL — Privilege Model Difference

| Aspect | MariaDB/MySQL | PostgreSQL |
|--------|--------------|------------|
| Superuser access | `root` user via password | `postgres` OS user via peer auth |
| Grant syntax | `GRANT ... ON db.* TO 'user'@'host'` | `GRANT ... ON DATABASE db TO user` |
| User scope | User tied to host (`'user'@'localhost'`) | User is global (no host binding) |
| Flush needed? | Yes — `FLUSH PRIVILEGES` | No — changes are immediate |
| Restart needed? | No | No |

---

## Understanding User Host Binding in MariaDB

In MariaDB/MySQL, a user is identified by **both username AND host**:

```sql
'kodekloud_gem'@'localhost'   -- can only connect from the same machine
'kodekloud_gem'@'%'           -- can connect from ANY host (less secure)
'kodekloud_gem'@'192.168.1.%' -- can connect from a specific subnet
```

This is why the full syntax is always `'user'@'host'` — not just `user`.

---

## GRANT Syntax in MariaDB

```sql
-- Grant all privileges on all tables in a specific database
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_gem'@'localhost';

-- Grant all privileges on everything (superuser-like, dangerous)
GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost';

-- Grant specific privileges
GRANT SELECT, INSERT, UPDATE, DELETE ON kodekloud_db5.* TO 'user'@'localhost';

-- Revoke privileges
REVOKE ALL PRIVILEGES ON kodekloud_db5.* FROM 'user'@'localhost';
```

> The `.*` after the database name means "all tables in this database".

---

## Why `FLUSH PRIVILEGES`?

MariaDB caches grant tables in memory. `FLUSH PRIVILEGES` forces a reload from disk:
- **Required** after direct edits to `mysql.user` table with `INSERT`/`UPDATE`
- **Not required** after `GRANT`/`CREATE USER` (these flush automatically)
- Best practice: run it anyway to be safe

---

## Key MariaDB Admin Commands

```sql
SHOW DATABASES;                          -- list all databases
SHOW TABLES;                             -- list tables in current db
USE dbname;                              -- switch to a database
SELECT user, host FROM mysql.user;       -- list all users
SHOW GRANTS FOR 'user'@'host';           -- show user's privileges
DROP USER 'user'@'host';                 -- delete a user
DROP DATABASE dbname;                    -- delete a database
ALTER USER 'user'@'host' IDENTIFIED BY 'newpass';  -- change password
```

---

## MariaDB Service Commands

```bash
sudo systemctl start mariadb       # start the service
sudo systemctl stop mariadb        # stop the service
sudo systemctl restart mariadb     # restart (drops all connections)
sudo systemctl reload mariadb      # reload config (graceful)
sudo systemctl enable mariadb      # start on boot
sudo systemctl status mariadb      # check current state
```

---

## `mysql_secure_installation` — What It Does

This script hardens a fresh MariaDB install by:

1. Setting (or changing) the root password
2. Removing anonymous users (created by default)
3. Disabling remote root login
4. Removing the test database (accessible to anonymous users)
5. Reloading privilege tables

Always run this on a fresh install before creating production databases.

---

## Troubleshooting Tips

| Problem | Check |
|---------|-------|
| `ERROR 1045: Access denied for user 'root'` | Use `sudo mysql` (root OS user bypasses password via socket auth) |
| `ERROR 1007: Can't create database; database exists` | Already exists — check with `SHOW DATABASES;` |
| `ERROR 1396: Operation CREATE USER failed` | User already exists — check with `SELECT user FROM mysql.user;` |
| New user can't connect | Check host binding — created as `'localhost'` but connecting remotely? Use `'%'` |
| `GRANT` not taking effect | Run `FLUSH PRIVILEGES;` |
| `systemctl start mariadb` fails | Check logs: `journalctl -xe` or `cat /var/log/mariadb/mariadb.log` |

---

## Day 17 vs Day 18 — Side-by-Side Comparison

| Step | PostgreSQL (Day 17) | MariaDB (Day 18) |
|------|---------------------|------------------|
| Switch user | `sudo su - postgres` | `sudo mysql -u root` |
| Create user | `CREATE USER name WITH PASSWORD '...'` | `CREATE USER 'name'@'localhost' IDENTIFIED BY '...'` |
| Create DB | `CREATE DATABASE name;` | `CREATE DATABASE name;` |
| Grant | `GRANT ALL PRIVILEGES ON DATABASE db TO user` | `GRANT ALL PRIVILEGES ON db.* TO 'user'@'host'` |
| Flush | Not needed | `FLUSH PRIVILEGES;` |
| Verify | `\du` / `\l` | `SELECT user FROM mysql.user;` / `SHOW DATABASES;` |