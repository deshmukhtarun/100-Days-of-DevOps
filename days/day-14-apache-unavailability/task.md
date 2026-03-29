# Task: Day 14 - Troubleshoot Apache Unavailability and Port Reconfiguration

## Scenario
A monitoring tool reported Apache service unavailability on one of the app servers in Stratos DC. The goal is to identify the faulty host, ensure Apache is running on all app servers (stapp01, stapp02, stapp03), and reconfigure the service to listen on port 8084.

## Requirements
- Identify the faulty app host where Apache is down.
- Fix the issue and ensure Apache is active on all hosts.
- Reconfigure Apache to run on port 8084 across all app servers.
- Ensure the firewall allows traffic on port 8084.
- Validation: All app servers should respond to "curl http://<app-server>:8084".