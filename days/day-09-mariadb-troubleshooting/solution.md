# Solution

## Step 1 – Connect to Database Server

```
ssh peter@stdb01
```

---

## Step 2 – Check MariaDB Service Status

```
sudo systemctl status mariadb
```

Output showed:

* service was **inactive (dead)**
* failed to start

---

## Step 3 – Check Logs

```
sudo journalctl -xeu mariadb
```

No logs were found:

```
-- No entries --
```

This indicated that the service was not reaching the logging stage.

---

## Step 4 – Check Data Directory

```
ls -ld /var/lib/mysql
```

Output:

```
No such file or directory
```

This confirmed that the MariaDB data directory was missing.

---

## Step 5 – Initialize Database

```
sudo mariadb-install-db --user=mysql --datadir=/var/lib/mysql
```

This created the required system tables and database structure.

---

## Step 6 – Fix Permissions

```
sudo chown -R mysql:mysql /var/lib/mysql
```

Ensures MariaDB has proper access to its data directory.

---

## Step 7 – Start MariaDB Service

```
sudo systemctl start mariadb
```

---

## Step 8 – Verify Service

```
sudo systemctl status mariadb
```

Output:

```
active (running)
```

---

## Final Outcome

MariaDB service is now running successfully, and the application can connect to the database.
