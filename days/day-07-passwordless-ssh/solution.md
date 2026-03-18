# Solution

## Step 1 – Switch to thor User on Jump Host

```
sudo su - thor
```

---

## Step 2 – Generate SSH Key Pair

```
ssh-keygen -t rsa
```

Press **Enter** for all prompts (no passphrase).

This creates:

* private key → ~/.ssh/id_rsa
* public key → ~/.ssh/id_rsa.pub

---

## Step 3 – Copy Public Key to App Servers

### App Server 1

```
ssh-copy-id tony@app-server1
```

### App Server 2

```
ssh-copy-id steve@app-server2
```

### App Server 3

```
ssh-copy-id banner@app-server3
```

(Use correct usernames based on your lab setup.)

---

## Step 4 – Verify Password-less SSH

Try logging in:

```
ssh tony@app-server1
```

If setup is correct:

* You should log in **without entering a password**

---

## Step 5 – Repeat for All Servers

Ensure password-less login works for:

* app-server1
* app-server2
* app-server3

---

## Alternative Manual Method (If Needed)

If `ssh-copy-id` is not available:

```
cat ~/.ssh/id_rsa.pub | ssh tony@app-server1 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```