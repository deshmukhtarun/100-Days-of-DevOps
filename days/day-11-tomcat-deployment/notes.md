# Notes

## What is Tomcat?

Apache Tomcat is a Java-based web server used to deploy:

* Java web applications
* WAR (Web Archive) files

---

## WAR Deployment

A `.war` file is a packaged Java web application.

Deploying:

```
cp ROOT.war /usr/share/tomcat/webapps/
```

Tomcat automatically:

* extracts the archive
* serves it as a web application

---

## ROOT Application

Using:

```
ROOT.war
```

ensures:

```
http://server:port/
```

serves the application directly.

If a different name is used:

```
app.war → http://server:port/app
```

---

## Changing Port

Tomcat port is configured in:

```
/etc/tomcat/server.xml
```

Changing:

```
port="8080" → port="8083"
```

---

## Service Management

```
systemctl start tomcat
systemctl enable tomcat
```

---

## DevOps Context

This task represents a typical deployment workflow:

1. install application server
2. configure service
3. deploy artifact
4. verify application

In real-world setups, this is automated using:

* CI/CD pipelines
* Docker containers
* Kubernetes deployments

---

## Key Learning

This task combines:

* application deployment
* service configuration
* file transfer
* verification

These are essential DevOps skills.