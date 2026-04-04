# Day 16 — Nginx Load Balancer Configuration

## Task Description

Day by day traffic is increasing on one of the websites managed by the `Nautilus` production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e. on `Nautilus` infra in `Stratos DC`.

They started the migration last month and it is almost done, as only the LBR server configuration is pending.

## Requirements

- **a.** Install `nginx` on the `LBR` (load balancer) server if it is not already installed.
- **b.** Configure load-balancing with the `http` context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at `/etc/nginx/nginx.conf`.
- **c.** Do not update the apache port that is already defined in the apache configuration on all app servers. Also make sure apache service is up and running on all the app servers.
- **d.** Once done, you can access the website by running `curl http://stlb01:80` in the terminal.

## Environment

| Host     | Role              | User   | Password   |
|----------|-------------------|--------|------------|
| stapp01  | App Server 1      | tony   | Ir0nM@n    |
| stapp02  | App Server 2      | steve  | Am3ric@    |
| stapp03  | App Server 3      | banner | BigGr33n   |
| stlb01   | Load Balancer     | loki   | Mischi3f   |
