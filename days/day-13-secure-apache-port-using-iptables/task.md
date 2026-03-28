# Task: Day 13 - Secure Apache Port using Iptables

## Scenario
The security team has flagged that Apache port 5002 is open to everyone on the Nautilus infrastructure in Stratos DC. Since there is no firewall on these hosts, the goal is to add a security layer using iptables to restrict access.

## Requirements
- Install iptables and all dependencies on each app host (stapp01, stapp02, stapp03).
- Block incoming traffic on port 5002 for everyone except the LBR (Load Balancer) host.
- Ensure the rules are persistent across system reboots.
- Validation: Port 5002 should be unreachable from the jump-host but reachable from the LBR.