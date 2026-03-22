# Solution

## Step 1 – Connect to App Server 1

```
ssh tony@app-server1
```

---

## Step 2 – Install Tomcat

```
sudo yum install -y tomcat
```

---

## Step 3 – Configure Tomcat Port

Edit configuration:

```
sudo vi /etc/tomcat/server.xml
```

Find:

```
<Connector port="8080" protocol="HTTP/1.1"
```

Change to:

```
<Connector port="8083" protocol="HTTP/1.1"
```

Save and exit.

---

## Step 4 – Copy WAR File from Jump Host

From App Server 1:

```
scp thor@jump-host:/tmp/ROOT.war /tmp/
```

---

## Step 5 – Deploy Application

```
sudo cp /tmp/ROOT.war /usr/share/tomcat/webapps/
```

---

## Step 6 – Start and Enable Tomcat

```
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

---

## Step 7 – Verify Service

```
sudo systemctl status tomcat
```

Expected:

```
active (running)
```

---

## Step 8 – Test Application

```
curl http://stapp01:8083
```

Expected:

* application output should be displayed
* confirms deployment is successful

---

## Final Outcome

* Tomcat installed
* Running on port 8083
* Application deployed successfully
* Accessible via base URL