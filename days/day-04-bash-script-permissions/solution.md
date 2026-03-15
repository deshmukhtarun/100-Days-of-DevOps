# Solution

## Step 1 – Connect to App Server 2

```bash
ssh user@app-server2
```

---

## Step 2 – Grant Execute Permission

Set execute permission so that all users can run the script.

```bash
sudo chmod 755 /tmp/xfusioncorp.sh
```

Explanation:

- `chmod` → change file permissions
- `755` → sets permissions to `rwxr-xr-x`
- owner → read, write, execute
- group → read, execute
- others → read, execute

---

## Step 3 – Verify Permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected output:

```
-rwxr-xr-x 1 root root ... /tmp/xfusioncorp.sh
```

Explanation of permissions:

* `rwx` → owner can read, write, execute
* `r-x` → group can read and execute
* `r-x` → others can read and execute

This confirms that **all users are able to execute the script**.
