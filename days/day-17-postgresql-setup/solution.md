# Day 17 — Solution

## Step 1: SSH into the Database Server

```bash
ssh peter@stdb01
```

---

## Step 2: Switch to the `postgres` System User

PostgreSQL commands must be run as the `postgres` OS user, who has superuser privileges inside PostgreSQL.

```bash
sudo su - postgres
```

---

## Step 3: Open the PostgreSQL Interactive Shell

```bash
psql
```

You should see the `postgres=#` prompt.

---

## Step 4: Create the Database User with Password

```sql
CREATE USER kodekloud_tim WITH PASSWORD 'YchZHRcLkL';
```

---

## Step 5: Create the Database

```sql
CREATE DATABASE kodekloud_db5;
```

---

## Step 6: Grant Full Permissions to the User on the Database

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_tim;
```

---

## Step 7: Verify Everything (Optional but Recommended)

```sql
-- List all users
\du

-- List all databases
\l

-- Connect to the database and check grants
\c kodekloud_db5
\dp
```

---

## Step 8: Exit psql and Return to Normal User

```sql
\q
```

```bash
exit   -- exits postgres user, back to peter
```

---

## All Steps as a One-liner (Alternative)

You can run all SQL commands non-interactively:

```bash
sudo -u postgres psql -c "CREATE USER kodekloud_tim WITH PASSWORD 'YchZHRcLkL';"
sudo -u postgres psql -c "CREATE DATABASE kodekloud_db5;"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_tim;"
```

> ⚠️ Do NOT run `systemctl restart postgresql` — the task explicitly forbids restarting the service.
