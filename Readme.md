# Readme

## Description

*ansible-zabbix* is an [Ansible](http://ansible.cc) playbook.
The playbook contains tasks to install Zabbix server (with MySQL backend), the PHP frontend and Zabbix agent.

## Requirements

### Ansible

Everything you should know about Ansible is documented on the [Ansible](http://ansible.cc/docs/gettingstarted.html) site...

### Supported platforms

#### Debian wheezy

Playbook tested on *Debian-7.1*.

#### Ansible >= 1.3

Any Ansible version >= 1.3 should work. If not, please use the issue tracker to report any bugs.

## Usage

### Get the code

```bash
$ git clone git@github.ugent.be:Onderwijstechnologie/ansible-zabbix.git roles/zabbix
```

The code should reside in the roles directory of ansible ( See [ansible documentation](http://www.ansibleworks.com/docs/playbooks.html#roles) for more information on roles ), in a folder zabbix.

### Create an Ansible inventory file

Following example makes Ansible aware of a box reachable through SSH on 127.0.0.1, port 2222.

```bash
$ vi ansible.host
```

with

```
[zabbix]
127.0.0.1:2222
```

The *ansible-zabbix* playbook is only executed for the host/group *zabbix*.

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
# Install Zabbix agent?
with_zabbix_agent: true

# Zabbix server ip
with_zabbix_server_ip: 127.0.0.1

# Install Zabbix server?
with_zabbix_server: true
mysql:
  admin_user: root
  admin_password: root
  host: 127.0.0.1

# Install Zabbix web frontend?
with_zabbix_web: true

# Install Zabbix Java Gateway?
with_zabbix_java: true

# Add custom userparameter(s)
zabbix_userparameter:
  - 'key, command'
```

### Run the playbook

First create a playbook including the zabbix role, naming it zabbix.yml.

```yml
---
- hosts: zabbix
  roles:
    - zabbix
```

Use *ansible.host* as inventory. Run the playbook only for the remote host *zabbix*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -s -i ansible.host zabbix.yml --extra-vars="user=vagrant"
```

## Docs and contact

Read more on the Wiki pages about how this playbook works.

http://icto.ugent.be
