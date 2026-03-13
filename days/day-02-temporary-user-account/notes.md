# Notes

## Temporary User Accounts

Temporary user accounts are commonly created when developers, contractors, or support engineers require access for a limited time.

Instead of manually removing these accounts later, Linux allows administrators to set an **account expiration date**.

Once the expiration date is reached, the account automatically becomes inactive.

---

## Command Used

Create a user with expiration date:

```bash
useradd -e YYYY-MM-DD username
```

Example:

```bash
useradd -e 2027-03-28 mariyam
```

---

## Verifying Account Expiration

To check account aging and expiration details:

```bash
chage -l username
```

Example:

```bash
chage -l mariyam
```

This displays information such as:

* password expiry
* password change interval
* account expiration date

---

## Why Expiry Dates Are Important

Setting expiration dates improves **security and access management**.

Benefits include:

* automatic removal of temporary access
* reduced risk of forgotten user accounts
* better compliance with security policies

This is commonly used for:

* temporary developers
* contractors
* consultants
* project-based access
