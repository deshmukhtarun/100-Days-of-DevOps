# Solution

## Step 1 – Connect to App Server 2

```
ssh steve@app-server2
```

---

## Step 2 – Install zip Package

```
sudo yum install -y zip
```

---

## Step 3 – Create Scripts Directory

```
mkdir -p /scripts
```

---

## Step 4 – Create Script

```
vi /scripts/ecommerce_backup.sh
```

Add:

```
#!/bin/bash

# Variables
SOURCE="/var/www/html/ecommerce"
DEST="/backup/xfusioncorp_ecommerce.zip"

# Create zip archive
zip -r $DEST $SOURCE

# Copy to storage server
scp $DEST natasha@ststor01:/backup/
```

---

## Step 5 – Set Permissions

```
chmod 755 /scripts/ecommerce_backup.sh
```

---

## Step 6 – Setup Password-less SSH

```
ssh-keygen -t rsa
ssh-copy-id natasha@ststor01
```

---

## Step 7 – Run Script

```
/scripts/ecommerce_backup.sh
```

---

## Step 8 – Verify

### Local:

```
ls /backup/
```

Expected:

```
xfusioncorp_ecommerce.zip
```

---

### Remote:

```
ssh natasha@ststor01
ls /backup/
```

Expected:

```bash
xfusioncorp_ecommerce.zip
```

---

## Final Outcome

* Archive created successfully
* Stored locally in `/backup/`
* Copied to storage server
* Script runs without password
