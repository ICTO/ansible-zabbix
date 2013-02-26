# Readme

## Description

*ansible-zabbix* is an [Ansible](http://ansible.cc) playbook.
The playbook contains tasks to install Zabbix server (with MySQL backend), the PHP frontend and Zabbix agent.

## Requirements

### Ansible

Everything is documented on the [Ansible](http://ansible.cc/docs/gettingstarted.html) site...

### Supported platforms

Playbook tested on *Debian-7.0-b4-amd64*, probably works on other Debian versions too. Heavily depends on *apt*, sorry CentOS users...

## Usage

### Get the code

```bash
$ git clone git@github.ugent.be:tberton/ansible-zabbix.git
```

### Create an Ansible inventory file

Following example makes ansible aware of a box reachable on localhost port 2222.

```bash
$ vi ansible.host
```

with

```
[zabbix]
127.0.0.1 ansible_ssh_port=2222

[zabbix:vars]
with_zabbix_server_ip='127.0.0.1'
```

### Create group specific variables

Create a *group_vars* directory where the *ansible.host* file is located.

```bash
$ mkdir group_vars
```

Create the file *zabbix* in the newly created directory.

```bash
$ cd group_vars
$ vi zabbix
```

with

```
---
with_zabbix_agent: true
with_zabbix_server: true
with_zabbix_web: true

```

### Run the playbook

Use *ansible.host* as inventory. Run the playbook only for the remote host *zabbix*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -i ansible.host ansible-zabbix/setup.yml --extra-vars="user=vagrant"
```

## Docs and contact

Read more on the Wiki pages about how this playbook works.

http://icto.ugent.be
