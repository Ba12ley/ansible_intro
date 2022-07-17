# Ansible

## Install

*pip install ansible*

Once installed

*ansible localhost -a "echo 'hello''"*

### SSH

#### Mac/Linux

*ssh-keygen -t rsa -b 4096 -C email@email.com*

-t for type

-b number of bits

-C custom email if required

## Concepts

To perform actions modules are used.

git, service, postrgesql, redis are all have modules.

To use a module Tasks are used.

Roles are the tasks applied to an instance

Role (Web)
- Task
- Task

Playbooks house the Roles

The Inventory maps IP addresses and hostnames

Can be written in YAML or an .ini file

So going from different environments the same playbook can be ran with different a inventory (hosts)

### Modules

Provided by Ansible and generally written in Python, to perform specific action i.e. clone a github repo, start a redis server, enable a firewall or notifications

Ansible Documentation organised by category.

### Tasks

To use modules tasks must be envoked

Tasks are written in YAML

See example_task.yaml and below

name: ensure git is installed (Human readable explanation of the task)

apt: name=git-core state=present update_cache=yes (module: package name state=the state required update_cache=whether you want to update the versions)

become: true (envoke su privileges)

### Roles

Roles group ansible tasks together to make them reusable, regardless of environments

### Playbooks

Top level collection

Contain one or more roles, have many tasks, variables and other info for execution.

### Inventory/Hosts

Is a list of servers/devices to execute against and is typically grouped by a role

Default location is /etc/ansible/hosts.  To explicitly set the inventory file use *-i*



## Running a Task

*ansible localhost -m setup*

This will gather the facts about the system/host

*ansible localhost -a "name: ensure git is installed
  apt: name=git-core state=present update_cache=yes
  become: true" -b -K -e ansible_python_interpreter=/usr/bin/python3*

-b to use superuser privileges
-K force to ask for sudo password
-e explicit state the interpreter, stops using python2 and can be used in the hosts file.  See first_playbook

## Running Playbooks
 *ansible-playbook -i ./hosts --private-key=./id_rsa playbook.yml*

-i specifies the hosts

--private-key specifies the ssh key to use

followed by the playbook to run

-vvvv most verbose output, more v more verbose


### Setting Variables and templates

Setting variables in the group_vars directory in a file named all


## Encrypt sensitive files

Ansible vault

*ansible-vault encrypt {filename}*
Add a password, will be requested to edit files

To specify and editor

*EDITOR=nano ansible-vault edit {filename}*

To run play book

*ansible-playbook --ask-vault-pass -i {inventory} --private-key={key} {playbook.yml}*

## Running from a specific task
*ansible-playbook -i ./hosts --private-key=./id_rsa playbook.yml --start-at-task="task name in .yml file"*
