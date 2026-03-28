# Solution: Day 13 - Implementing Iptables Security

### Step 1: Identify the LBR Host IP
From the jump-host or app-server, find the internal IP for the load balancer (stlb01):
- Command: getent hosts stlb01
- Result: 10.244.244.141

### Step 2: Install and Prepare Iptables (Repeat for stapp01, stapp02, stapp03)
SSH into each app server and install both the service and the core binary:
- Command: sudo yum install -y iptables-services iptables
- Command: sudo systemctl enable iptables

### Step 3: Configure Firewall Rules
Manually apply the allow rule for the LBR IP followed by the reject rule for all others:
- Command: sudo iptables -A INPUT -p tcp -s 10.244.244.141 --dport 5002 -j ACCEPT
- Command: sudo iptables -A INPUT -p tcp --dport 5002 -j REJECT

### Step 4: Ensure Persistence
Save the current rules to the configuration file so they load automatically on boot, then start the service:
- Command: sudo iptables-save > /etc/sysconfig/iptables
- Command: sudo systemctl start iptables
- Command: sudo systemctl status iptables

### Step 5: Verification
From the jump-host, attempt to curl the app servers on port 5002:
- Command: curl http://stapp01:5002
- Command: curl http://stapp02:5002
- Command: curl http://stapp03:5002
- Result: All should return "Connection refused".