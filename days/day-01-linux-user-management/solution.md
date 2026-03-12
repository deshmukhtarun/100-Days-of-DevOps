# Solution

## Step 1 – Connect to the Server

SSH into the required server.

```bash
ssh user@app-server1
```

---

## Step 2 – Create the User with Non-Interactive Shell

```bash
sudo useradd -s /sbin/nologin rose
```

Explanation:

* `useradd` → command used to create a new user
* `-s` → defines the login shell
* `/sbin/nologin` → prevents interactive login
* `rose` → username

---

## Step 3 – Verify the User

Check if the user was created successfully.

```bash
cat /etc/passwd | grep rose
```

Example output:

```
rose:x:1005:1005::/home/rose:/sbin/nologin
```

This confirms that the user exists and the shell is set to **/sbin/nologin**, making it non-interactive.
