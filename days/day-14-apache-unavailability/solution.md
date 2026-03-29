# Solution: Day 14 - Apache Troubleshooting and Port Migration

### Step 1: Identify the Faulty Host
From the jump-host, check port 8084 on all app servers:
- Command: curl http://stapp01:8084
- Observation: stapp01 failed to connect, while others responded (or were on the wrong port).

### Step 2: Resolve Port Conflict (stapp01)
SSH into stapp01 and identify what is blocking port 8084:
- Command: sudo ss -tunlp | grep :8084
- Observation: 'sendmail' was listening on 127.0.0.1:8084.
- Action: Kill the conflicting process:
- Command: sudo kill -9 <PID_OF_SENDMAIL>

### Step 3: Configure Apache Port
Update the Listen directive in the Apache configuration file:
- File: /etc/httpd/conf/httpd.conf
- Change: Update "Listen 80" to "Listen 8084".
- Note: Ensure no syntax errors like "Listen 808484" occur during automated replacement.
- Command: sudo apachectl configtest (to verify syntax)
- Command: sudo systemctl restart httpd
- Command: sudo systemctl enable httpd

### Step 4: Configure Firewall
Install iptables and allow the new port 8084:
- Command: sudo yum install -y iptables
- Command: sudo iptables -I INPUT -p tcp --dport 8084 -j ACCEPT
- Command: sudo iptables-save | sudo tee /etc/sysconfig/iptables

### Step 5: Final Verification
From the jump-host, verify all servers:
- Command: curl http://stapp01:8084
- Command: curl http://stapp02:8084
- Command: curl http://stapp03:8084