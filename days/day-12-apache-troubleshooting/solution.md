# Solution: Day 12 - Apache Troubleshooting

### Step 1: Access the Target Server
Log into the app server stapp01 from the jump host and switch to the root user:
- Command: ssh tony@stapp01
- Command: sudo su -

### Step 2: Diagnose the Service Failure
Check the status of the Apache (httpd) service:
- Command: systemctl status httpd
- Observation: The service failed to start with the error: "(98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:3004".

### Step 3: Identify and Clear the Conflicting Port
Since port 3004 is already occupied by another process, find the Process ID (PID) using it:
- Command: ss -tunlp | grep 3004
- Action: Kill the conflicting process to free up the port:
- Command: kill -9 <PID_FROM_OUTPUT>

### Step 4: Configure and Start Apache
Check the configuration file to ensure it's set to listen on 3004:
- Command: grep "Listen" /etc/httpd/conf/httpd.conf
- Action: If not set, edit the file and update to "Listen 3004".
- Action: Start the service and enable it:
- Command: systemctl start httpd
- Command: systemctl enable httpd

### Step 5: Fix Firewall Rules (iptables)
The jump host was receiving "No route to host," indicating an iptables REJECT rule. Insert an allow rule for port 3004 at the top of the INPUT chain:
- Command: iptables -I INPUT -p tcp --dport 3004 -j ACCEPT

### Step 6: Final Verification
From the jump-host, verify the connection:
- Command: curl http://stapp01:3004