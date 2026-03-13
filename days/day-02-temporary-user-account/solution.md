# Solution

## Step 1 – Connect to App Server 3

```bash
ssh user@app-server3
```

---

## Step 2 – Create the User with Expiry Date

```bash
sudo useradd -e 2027-03-28 mariyam
```

Explanation:

* `useradd` → command used to create a new user
* `-e` → sets an account expiry date
* `2027-03-28` → date when the account will expire
* `mariyam` → username (must be lowercase)

---

## Step 3 – Verify User Account Expiry

Check account details using:

```bash
sudo chage -l mariyam
```

Example output:

```
Account expires : Mar 28, 2027
```

This confirms the user account has been created with the correct expiration date.
