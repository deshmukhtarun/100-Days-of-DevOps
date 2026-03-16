# Solution

## Step 1 – Connect to App Server 2

```bash
ssh user@app-server2
```

---

## Step 2 – Install SELinux Packages

Install the required SELinux utilities.

```bash
sudo yum install -y selinux-policy selinux-policy-targeted
```

These packages provide the SELinux policy and management tools.

---

## Step 3 – Disable SELinux Permanently

Edit the SELinux configuration file.

```bash
sudo vi /etc/selinux/config
```

Find the following line:

```
SELINUX=enforcing
```

Change it to:

```
SELINUX=disabled
```

Save and exit the file.

---

## Step 4 – Verify Configuration

Check the configuration file:

```bash
cat /etc/selinux/config
```

Expected configuration:

```
SELINUX=disabled
SELINUXTYPE=targeted
```

The change will take effect **after the next reboot**, which is already scheduled.
