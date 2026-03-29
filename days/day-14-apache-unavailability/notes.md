# DevOps Concept Notes: Port Conflicts and Syntax Validation

### 1. Identifying Port Squatters
When a service fails with "Address already in use," it means another process is bound to that port. 
- Use "ss -tunlp" to find the culprit.
- Common conflicts include system services like 'sendmail' or 'postfix' being pre-configured on non-standard ports.

### 2. Configuration Safety
Using automated tools like "sed" for configuration changes can lead to "double-replacing" (e.g., turning 80 into 808484).
- Always verify configuration syntax with "apachectl configtest" or "httpd -t" before restarting the service to avoid downtime.



### 3. Firewall Alignment
Changing a service port (e.g., 80 to 8084) requires an immediate corresponding change in the firewall. If you move a service but don't update iptables/firewalld, the service will appear "down" to external monitoring tools even if it is running perfectly on the local host.

### 4. Service Persistence
Always combine "systemctl start" with "systemctl enable". The former starts the service now; the latter ensures it starts after the next maintenance reboot.