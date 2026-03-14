# Solution

## Step 1 – Connect to the App Server

SSH into each application server.

```bash
ssh user@app-server1
```

Repeat the process for all app servers in the Stratos Datacenter.

---

## Step 2 – Edit the SSH Configuration File

Open the SSH daemon configuration file.

```bash
sudo vi /etc/ssh/sshd_config
```

Locate the following line:

```
PermitRootLogin yes
```

Change it to:

```
PermitRootLogin no
```

If the line is commented out, ensure it is uncommented and set to **no**.

---

## Step 3 – Restart the SSH Service

Apply the configuration changes by restarting the SSH service.

```bash
sudo systemctl restart sshd
```

---

## Step 4 – Verify Configuration

Confirm the setting was applied:

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

Expected output:

```
PermitRootLogin no
```

This confirms that direct root SSH login has been successfully disabled.
