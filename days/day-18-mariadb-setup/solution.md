# Day 18 — Solution

## Step 1: SSH into the Database Server

```bash
ssh peter@stdb01
```

---

## Step 2: Install MariaDB Server

```bash
sudo yum install -y mariadb-server      # RHEL/CentOS
# OR
sudo apt install -y mariadb-server      # Ubuntu/Debian
```

---

## Step 3: Start and Enable MariaDB Service

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

---

## Step 4: Secure the Installation (Recommended)

Run the built-in security script to set root password and remove anonymous users:

```bash
sudo mysql_secure_installation
```

When prompted:
- Enter current root password → press **Enter** (none set yet)
- Set root password? → **Y**, then enter a password
- Remove anonymous users? → **Y**
- Disallow root login remotely? → **Y**
- Remove test database? → **Y**
- Reload privilege tables? → **Y**

> If the task environment already has root access without a password, you can skip this step and go straight to Step 5.

---

## Step 5: Log into MariaDB as Root

```bash
sudo mysql -u root
# OR if root password was set during secure installation:
sudo mysql -u root -p
```

---

## Step 6: Create the Database

```sql
CREATE DATABASE kodekloud_db5;
```

---

## Step 7: Create the User with Password

```sql
CREATE USER 'kodekloud_gem'@'localhost' IDENTIFIED BY 'TmPcZjtRQx';
```

---

## Step 8: Grant Full Permissions on the Database

```sql
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_gem'@'localhost';
```

---

## Step 9: Flush Privileges & Verify

```sql
FLUSH PRIVILEGES;

-- Verify user exists
SELECT user, host FROM mysql.user WHERE user = 'kodekloud_gem';

-- Verify database exists
SHOW DATABASES;

-- Verify grants
SHOW GRANTS FOR 'kodekloud_gem'@'localhost';
```

---

## Step 10: Exit MariaDB

```sql
EXIT;
```

---

## All SQL Steps as One-liners (Alternative)

```bash
sudo mysql -u root -e "CREATE DATABASE kodekloud_db5;"
sudo mysql -u root -e "CREATE USER 'kodekloud_gem'@'localhost' IDENTIFIED BY 'TmPcZjtRQx';"
sudo mysql -u root -e "GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_gem'@'localhost';"
sudo mysql -u root -e "FLUSH PRIVILEGES;"
```

---

## Step 11: Test Login as New User

```bash
mysql -u kodekloud_gem -pTmPcZjtRQx kodekloud_db5
```

You should land inside the `kodekloud_db5` database — confirming everything works.