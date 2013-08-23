# Readme

## Description

*ansible-zabbix* is an [Ansible](http://ansible.cc) playbook.
The playbook contains tasks to install Zabbix server (with MySQL backend), the PHP frontend and Zabbix agent.

## Requirements

### Ansible

Everything you should know about Ansible is documented on the [Ansible](http://ansible.cc/docs/gettingstarted.html) site...

### Supported platforms

#### Debian wheezy

Playbook tested on *Debian-7.1*, probably works on other Debian versions too. Heavily depends on *apt*, sorry CentOS users...

#### Ansible >= 1.2

Any Ansible version >= 1.2 should work. If not, please use the issue tracker to report any bugs.

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
# Install Zabbix agent?
with_zabbix_agent: true

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
```

### Run the playbook

Use *ansible.host* as inventory. Run the playbook only for the remote host *zabbix*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -i ansible.host ansible-zabbix/setup.yml --extra-vars="user=vagrant"
```

### Example output

```
SSH password: 

PLAY [Zabbix playbook] ********************* 

GATHERING FACTS ********************* 
ok: [127.0.0.1]

TASK: [Fail if no zabbix server ip defined] ********************* 
skipping: [127.0.0.1]

TASK: [Get and install zabbix gpg key] ********************* 
skipping: [127.0.0.1]

TASK: [install python-software-properties] ********************* 
skipping: [127.0.0.1]

TASK: [add repo to sources] ********************* 
skipping: [127.0.0.1]

TASK: [update cache] ********************* 
skipping: [127.0.0.1]

TASK: [Install Zabbix agent dependencies] ********************* 
changed: [127.0.0.1] => (item=libcurl3-gnutls)

TASK: [Fetch Debian package: zabbix-agent_2.0.5+dfsg-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix agent: zabbix-agent_2.0.5+dfsg-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Put custom Zabbix agent settings in include dir] ********************* 
changed: [127.0.0.1]

TASK: [Ensure Zabbix agent is running] ********************* 
ok: [127.0.0.1]

TASK: [Install MySQL server] ********************* 
8changed: [127.0.0.1]

TASK: [Change MySQL root password] ********************* 
changed: [127.0.0.1]

TASK: [Install MySQLdb python package] ********************* 
changed: [127.0.0.1]

TASK: [Manage Zabbix db: zabbix] ********************* 
changed: [127.0.0.1]

TASK: [Manage Zabbix db-user: zabbix] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix server dependencies] ********************* 
changed: [127.0.0.1] => (item=fping)
changed: [127.0.0.1] => (item=libiksemel3)
changed: [127.0.0.1] => (item=libopenipmi0)
changed: [127.0.0.1] => (item=libperl5.14)
changed: [127.0.0.1] => (item=libsensors4)
changed: [127.0.0.1] => (item=libsnmp-base)
changed: [127.0.0.1] => (item=libsnmp15)
ok: [127.0.0.1] => (item=libmysqlclient18)
ok: [127.0.0.1] => (item=mysql-common)
changed: [127.0.0.1] => (item=libiodbc2)
ok: [127.0.0.1] => (item=libssl1.0.0)
ok: [127.0.0.1] => (item=libssl-dev)
changed: [127.0.0.1] => (item=dbconfig-common)

TASK: [Fetch Debian package: zabbix-server-mysql_2.0.5+dfsg-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix server: zabbix-server-mysql_2.0.5+dfsg-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Execute Zabbix SQL files] ********************* 
changed: [127.0.0.1] => (item=schema)
changed: [127.0.0.1] => (item=images)
changed: [127.0.0.1] => (item=data)

TASK: [Create Zabbix server include directory] ********************* 
changed: [127.0.0.1]

TASK: [Enable Zabbix server include directory] ********************* 
changed: [127.0.0.1]

TASK: [Enable Zabbix server in /etc/default/zabbix-server] ********************* 
changed: [127.0.0.1]

TASK: [Put custom Zabbix server settings in include dir] ********************* 
changed: [127.0.0.1]

TASK: [Ensure Zabbix server is running] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix PHP frontend dependencies] ********************* 
changed: [127.0.0.1] => (item=apache2)
changed: [127.0.0.1] => (item=php5)
ok: [127.0.0.1] => (item=libapache2-mod-php5)
changed: [127.0.0.1] => (item=php5-mysql)
changed: [127.0.0.1] => (item=php5-gd)
ok: [127.0.0.1] => (item=ttf-dejavu-core)

TASK: [Fetch Debian package: zabbix-frontend-php_2.0.5+dfsg-1_all.deb] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix PHP frontend: zabbix-frontend-php_2.0.5+dfsg-1_all.deb] ********************* 
changed: [127.0.0.1]

TASK: [Link Apache config file] ********************* 
changed: [127.0.0.1]

TASK: [Put Zabbix web frontend config in place] ********************* 
changed: [127.0.0.1]

TASK: [Put custom Zabbix PHP settings in include dir] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix java dependencies] ********************* 
changed: [127.0.0.1] => (item=openjdk-6-jre)

TASK: [Fetch Debian package: zabbix-java-gateway_2.0.5-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Install Zabbix java: zabbix-java-gateway_2.0.5-1_amd64.deb] ********************* 
changed: [127.0.0.1]

TASK: [Ensure Zabbix Java Gateway is running] ********************* 
ok: [127.0.0.1]

NOTIFIED: [Restart Zabbix agent] ********************* 
changed: [127.0.0.1]

NOTIFIED: [Restart Zabbix server] ********************* 
changed: [127.0.0.1]

NOTIFIED: [Restart Apache] ********************* 
changed: [127.0.0.1]

PLAY RECAP ********************* 
127.0.0.1                   : ok=33   changed=30   unreachable=0    failed=0  PLAY [Zabbix playbook] ********************* 
```

## Docs and contact

Read more on the Wiki pages about how this playbook works.

http://icto.ugent.be
