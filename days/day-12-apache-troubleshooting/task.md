# Task: Troubleshoot Apache Service in Stratos Datacenter

## Scenario
Monitoring reported that an app server in the Stratos Datacenter is unreachable on port `3004`. The objective is to identify if the service is down, the firewall is misconfigured, or another issue exists, and fix it.

## Requirements
- Identify why Apache is not reachable on port `3004`.
- Fix the issue so Apache is reachable from the `jump-host`.
- Do not alter the existing `index.html` code.
- Validate using `curl http://stapp01:3004` from the jump host.