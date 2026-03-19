# Solution

## Step 1 – Install pip3 (if not already installed)

```
sudo yum install -y python3-pip
```

---

## Step 2 – Install Ansible Using pip3

```
sudo pip3 install ansible==4.7.0
```

This installs the required version globally.

---

## Step 3 – Verify Installation

```
ansible --version
```

Expected output should show:

```
ansible [core ...]
```

and version corresponding to **4.7.0**

---

## Step 4 – Ensure Global Availability

Since installation was done using `sudo pip3`, binaries are placed in:

```
/usr/local/bin/
```

This directory is part of the system PATH, making Ansible accessible to all users.

Verify:

```
which ansible
```

Expected output:

```
/usr/local/bin/ansible
```
