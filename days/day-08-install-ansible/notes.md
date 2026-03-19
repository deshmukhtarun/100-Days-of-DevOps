# Notes

## What is Ansible?

Ansible is an open-source automation tool used for:

* configuration management
* application deployment
* infrastructure provisioning
* orchestration

It is **agentless**, meaning no software needs to be installed on target servers.

---

## Why pip3 Installation?

Ansible can be installed using:

* package managers (yum/apt)
* pip (Python package manager)

In this task, pip3 is required to:

* install a specific version (4.7.0)
* ensure flexibility and version control

---

## Global Installation

Using:

```
sudo pip3 install ansible
```

ensures:

* binaries are placed in `/usr/local/bin`
* accessible by all users
* no need for virtual environments

---

## Verify Binary Location

```
which ansible
```

This confirms where the executable is located.

---

## DevOps Context

Ansible is widely used in real-world DevOps for:

* server configuration
* deployment automation
* infrastructure as code
* CI/CD pipelines

It operates over SSH and relies heavily on:

* inventory files
* playbooks (YAML)
* modules

---

## Key Advantage

Unlike tools like Puppet or Chef, Ansible:

* requires no agents
* uses simple YAML syntax
* works over SSH

This makes it ideal for quick setup and testing environments.
