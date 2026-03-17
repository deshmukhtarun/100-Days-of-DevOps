# Notes

## What is Cron?

Cron is a time-based job scheduler in Linux used to automate repetitive tasks such as:

* backups
* log rotation
* system maintenance
* script execution

---

## Cron Service

The cron daemon runs as:

```
crond
```

It must be running for scheduled jobs to execute.

---

## Crontab Command

Edit cron jobs:

```
crontab -e
```

List cron jobs:

```
crontab -l
```

---

## Cron Schedule Format

```
* * * * * command
│ │ │ │ │
│ │ │ │ └── day of week (0–7)
│ │ │ └──── month (1–12)
│ │ └────── day of month (1–31)
│ └──────── hour (0–23)
└────────── minute (0–59)
```

---

## Example Used

```
*/5 * * * * echo hello > /tmp/cron_text
```

Meaning:

* runs every 5 minutes
* writes "hello" to a file

---

## Why Root Cron?

Some tasks require elevated privileges such as:

* system-level scripts
* service management
* backups

Root cron ensures the job has full system access.

---

## DevOps Context

Cron jobs are widely used in:

* automation scripts
* database backups
* CI/CD pipelines
* monitoring and alerting systems

In modern environments, cron is often replaced or complemented by:

* Kubernetes CronJobs
* CI/CD schedulers
* cloud-based schedulers
