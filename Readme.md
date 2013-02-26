# Readme

## Description

*ansible-zabbix* is an [Ansible](http://ansible.cc) playbook.
The playbook contains tasks to install Zabbix server (with MySQL backend), the PHP frontend and Zabbix agent.

## Requirements

### Ansible

Everything you should know about Ansible is documented on the [Ansible](http://ansible.cc/docs/gettingstarted.html) site...

### Supported platforms

#### Debian wheezy

Playbook tested on *Debian-7.0-b4-amd64*, probably works on other Debian versions too. Heavily depends on *apt*, sorry CentOS users...

#### Ansible >= 1.0

Any Ansible version >= 1.0 should work. If not, please use the issue tracker to report any bugs.

## Usage

### Get the code

```bash
$ git clone git@github.ugent.be:tberton/ansible-zabbix.git
```

### Create an Ansible inventory file

Following example makes Ansible aware of a box reachable through SSH on 127.0.0.1, port 2222.

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
with_zabbix_agent: true
with_zabbix_server: true
with_zabbix_web: true

```

### Run the playbook

Use *ansible.host* as inventory. Run the playbook only for the remote host *zabbix*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -i ansible.host ansible-zabbix/setup.yml --extra-vars="user=vagrant"
```

### Example output

```
PLAY [zabbix] ********************* 

GATHERING FACTS ********************* 
ok: [127.0.0.1]

TASK: [Create Zabbix agent system group: zabbix] ********************* 
ok: [127.0.0.1]

TASK: [Create Zabbix agent system user: zabbix] ********************* 
ok: [127.0.0.1]

TASK: [Transfer Zabbix agent package: zabbix-agent-2.0.4-1_amd64.deb to /tmp] ********************* 
ok: [127.0.0.1]

TASK: [Install Zabbix agent: zabbix-agent-2.0.4-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix agent init script] ********************* 
ok: [127.0.0.1]

TASK: [Enable Zabbix agent config include dir] ********************* 
ok: [127.0.0.1]

TASK: [Put custom Zabbix agent settings in include dir] ********************* 
ok: [127.0.0.1]

TASK: [Create Zabbix agent pid directory: /var/run/zabbix-agent] ********************* 
ok: [127.0.0.1]

TASK: [Create Zabbix agent log directory: /var/log/zabbix-agent] ********************* 
ok: [127.0.0.1]

TASK: [Create Zabbix agent logfile: /var/log/zabbix-agent/zabbix_agentd.log] ********************* 
changed: [127.0.0.1]

TASK: [Ensure correct permissions on Zabbix agent logfile: /var/log/zabbix-agent/zabbix_agentd.log] ********************* 
ok: [127.0.0.1]

TASK: [Trigger Zabbix agent restart] ********************* 
skipping: [127.0.0.1]

TASK: [Ensure Zabbix agent is running] ********************* 
ok: [127.0.0.1]

TASK: [Install MySQL server] ********************* 
skipping: [127.0.0.1]

TASK: [Install MySQLdb python package] ********************* 
skipping: [127.0.0.1]

TASK: [Manage Zabbix db: zabbix] ********************* 
skipping: [127.0.0.1]

TASK: [Manage Zabbix db-user: zabbix] ********************* 
skipping: [127.0.0.1]

TASK: [Transfer Zabbix SQL files] ********************* 
skipping: [127.0.0.1] => (item=schema.sql)
skipping: [127.0.0.1] => (item=images.sql)
skipping: [127.0.0.1] => (item=data.sql)

TASK: [Execute Zabbix SQL files] ********************* 
skipping: [127.0.0.1] => (item=schema.sql)
skipping: [127.0.0.1] => (item=images.sql)
skipping: [127.0.0.1] => (item=data.sql)

TASK: [Install MySQL clientlib dev package] ********************* 
skipping: [127.0.0.1]

TASK: [Create Zabbix server system group: zabbixsrv] ********************* 
skipping: [127.0.0.1]

TASK: [Create Zabbix server system user: zabbixsrv] ********************* 
skipping: [127.0.0.1]

TASK: [Transfer Zabbix server package: zabbix-server-mysql-2.0.4-1_amd64.deb to /tmp] ********************* 
skipping: [127.0.0.1]

TASK: [Install Zabbix server: zabbix-server-mysql-2.0.4-1_amd64.deb] ********************* 
skipping: [127.0.0.1]

TASK: [Install Zabbix agent init script] ********************* 
skipping: [127.0.0.1]

TASK: [Enable Zabbix server config include dir] ********************* 
skipping: [127.0.0.1]

TASK: [Put custom Zabbix server settings in include dir] ********************* 
skipping: [127.0.0.1]

TASK: [Create Zabbix server pid directory: /var/run/zabbix-server] ********************* 
skipping: [127.0.0.1]

TASK: [Create Zabbix server log directory: /var/log/zabbix-server] ********************* 
skipping: [127.0.0.1]

TASK: [Create Zabbix server logfile: /var/log/zabbix-server/zabbix_server.log] ********************* 
skipping: [127.0.0.1]

TASK: [Ensure correct permissions on Zabbix server logfile: /var/log/zabbix-server/zabbix_server.log] ********************* 
skipping: [127.0.0.1]

TASK: [Trigger Zabbix server restart] ********************* 
skipping: [127.0.0.1]

TASK: [Ensure Zabbix server is running] ********************* 
skipping: [127.0.0.1]

TASK: [Install Zabbix PHP frontend dependencies] ********************* 
skipping: [127.0.0.1] => (item=apache2,libapache2-mod-php5,php5-mysql,php5-gd)

TASK: [Transfer Zabbix web package: zabbix-frontend-php-2.0.4-1_amd64.deb to /tmp] ********************* 
skipping: [127.0.0.1]

TASK: [Install Zabbix PHP frontend: zabbix-frontend-php-2.0.4-1_amd64.deb] ********************* 
skipping: [127.0.0.1]

TASK: [Put Zabbix web frontend config in place] ********************* 
skipping: [127.0.0.1]

TASK: [Put custom Zabbix PHP settings in include dir] ********************* 
skipping: [127.0.0.1]

PLAY RECAP ********************* 
127.0.0.1                   : ok=15   changed=2    unreachable=0    failed=0    
```

## Docs and contact

Read more on the Wiki pages about how this playbook works.

http://icto.ugent.be
