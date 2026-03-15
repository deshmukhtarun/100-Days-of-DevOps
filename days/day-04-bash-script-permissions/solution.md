# Solution

## Step 1 – Connect to App Server 2

```bash
ssh user@app-server2
```

---

## Step 2 – Grant Execute Permission

Make the script executable for all users.

```bash
sudo chmod +x /tmp/xfusioncorp.sh
```

This adds execute permission to:

* owner
* group
* others

---

## Step 3 – Verify Permissions

Check the script permissions.

```bash
ls -l /tmp/xfusioncorp.sh
```

Example output:

```
-rwxr-xr-x 1 root root 1024 Mar 15 10:20 /tmp/xfusioncorp.sh
```

Explanation of permissions:

* `rwx` → owner can read, write, execute
* `r-x` → group can read and execute
* `r-x` → others can read and execute

This confirms that **all users are able to execute the script**.
