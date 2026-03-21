# Day 10 – Ecommerce Backup Script

## Task

The xFusionCorp Industries production team needs to automate website backups.

A static website is running on **App Server 2**, and a script must be created to back up the website data and store it locally as well as on the storage server.

## Requirements

1. Create a script named `ecommerce_backup.sh`
2. Place it under `/scripts` on App Server 2
3. The script should:

   * Create a zip archive of `/var/www/html/ecommerce`
   * Archive name: `xfusioncorp_ecommerce.zip`
   * Store it in `/backup/`
   * Copy it to the storage server `/backup/`
4. Script must:

   * NOT use `sudo`
   * NOT ask for password
   * be executable by users

Note: Install `zip` manually outside the script.
