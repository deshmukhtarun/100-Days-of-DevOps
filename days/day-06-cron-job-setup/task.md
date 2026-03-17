# Day 06 – Install Cron and Schedule Job

## Task

The Nautilus system admins team wants to automate routine tasks using scheduled jobs.

Before deploying production scripts, they need to test functionality using a sample cron job.

## Requirements

1. Install the **cronie package** on all Nautilus app servers.
2. Start the **crond service**.
3. Add the following cron job for the **root user**:

```
*/5 * * * * echo hello > /tmp/cron_text
```

This job should execute every 5 minutes.
