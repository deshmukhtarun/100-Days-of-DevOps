# Day 17 — Concepts & Notes

## Topic: PostgreSQL User & Database Management

---

## PostgreSQL vs MySQL — Key Difference in Access

In PostgreSQL, there is a special OS-level user called `postgres` that maps to the PostgreSQL superuser. Unlike MySQL where you connect as root with a password, PostgreSQL uses **peer authentication** by default for local connections — meaning the OS user must match the database user.

```
OS user: postgres  →  DB superuser: postgres  (peer auth, no password needed locally)
```

That's why we always do `sudo su - postgres` before running `psql`.

---

## Essential `psql` Commands

| Command | Description |
|---------|-------------|
| `\du` | List all database users/roles |
| `\l` | List all databases |
| `\c dbname` | Connect to a database |
| `\dt` | List tables in current database |
| `\dp` | Show access privileges |
| `\q` | Quit psql |
| `\?` | Help for psql meta-commands |
| `\h COMMAND` | SQL syntax help (e.g. `\h CREATE USER`) |

---

## Key SQL Commands Used

```sql
-- Create a user with password
CREATE USER username WITH PASSWORD 'password';

-- Create a database
CREATE DATABASE dbname;

-- Grant all privileges on a database to a user
GRANT ALL PRIVILEGES ON DATABASE dbname TO username;

-- Drop a user (cleanup)
DROP USER username;

-- Drop a database (cleanup)
DROP DATABASE dbname;

-- Change a user's password
ALTER USER username WITH PASSWORD 'newpassword';
```

---

## PostgreSQL Roles vs Users

In PostgreSQL, **users and roles are the same thing**. The only difference:
- `CREATE USER` = `CREATE ROLE` + `LOGIN` privilege (can log in by default)
- `CREATE ROLE` = cannot log in by default

```sql
-- These are equivalent:
CREATE USER kodekloud_tim WITH PASSWORD 'YchZHRcLkL';
CREATE ROLE kodekloud_tim WITH LOGIN PASSWORD 'YchZHRcLkL';
```

---

## PostgreSQL Authentication Methods (pg_hba.conf)

The file `/etc/postgresql/<version>/main/pg_hba.conf` (or `/var/lib/pgsql/data/pg_hba.conf` on RHEL) controls how clients authenticate.

| Method | Description |
|--------|-------------|
| `peer` | OS username must match DB username (local only) |
| `md5` | Password-based (hashed) |
| `scram-sha-256` | Stronger password auth (PostgreSQL 10+) |
| `trust` | No password required (dangerous in prod) |
| `reject` | Always deny |

> This is why we didn't need to change any config or restart — the database and user were created in-memory and persisted without a restart.

---

## GRANT Privilege Levels

```sql
-- On a database (top-level access)
GRANT ALL PRIVILEGES ON DATABASE dbname TO username;

-- On all tables in a schema (needed for full table access)
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO username;

-- On a specific table
GRANT SELECT, INSERT, UPDATE, DELETE ON tablename TO username;
```

> Note: `GRANT ALL ON DATABASE` gives connection + schema-level rights, but for full table access inside the DB you may also need `GRANT ALL ON ALL TABLES IN SCHEMA public`.

---

## Why We Don't Restart PostgreSQL

PostgreSQL holds all user/database metadata in its **system catalog** (internal tables like `pg_user`, `pg_database`). Changes via SQL (`CREATE USER`, `CREATE DATABASE`, `GRANT`) are:
- **Transactional** — committed immediately
- **Persistent** — written to disk (data directory)
- **Live** — no reload or restart required

Restarting is only needed for changes to config files like `postgresql.conf` or `pg_hba.conf`.

---

## Troubleshooting Tips

| Problem | Check |
|---------|-------|
| `psql: FATAL: role "peter" does not exist` | You're not switched to `postgres` user — use `sudo su - postgres` first |
| `ERROR: database already exists` | Database was already created — check with `\l` |
| `ERROR: role already exists` | User already exists — check with `\du` |
| `GRANT` seems to have no effect | Also run `GRANT ALL ON ALL TABLES IN SCHEMA public TO user;` inside the DB |
| Can't connect as new user | Check `pg_hba.conf` auth method and ensure `LOGIN` privilege on the role |
