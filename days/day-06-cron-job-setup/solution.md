# Solution

## Step 1 – Connect to App Servers

SSH into each app server.

```
ssh user@app-server1
```

Repeat for all app servers.

---

## Step 2 – Install Cronie Package

```
sudo yum install -y cronie
```

---

## Step 3 – Start and Enable crond Service

```
sudo systemctl start crond
sudo systemctl enable crond
```

---

## Step 4 – Add Cron Job for Root User

Edit root user's crontab:

```
sudo crontab -e
```

Add the following line:

```
*/5 * * * * echo hello > /tmp/cron_text
```

Save and exit.

---

## Step 5 – Verify Cron Job

List cron jobs:

```
sudo crontab -l
```

Wait a few minutes and verify output:

```
cat /tmp/cron_text
```

Expected output:

```
hello
```
