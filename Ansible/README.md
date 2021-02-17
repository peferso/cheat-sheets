# Ansible

## Ansible hosts

The target servers that will be configured running playbooks should be included in the Ansible inventory ```/etc/ansible/hosts```:

```sh
[dbec2server]

10.10.3.4

10.10.3.56

...
```

## Pre-requisites: allow passwordless ssh connection from Ansible server to target host

The Ansible server public ssh key should be added to each target db_server.

This is done first creating the ssh key pair:
```sh
$ ssh-keygen -t rsa
``` 

Then, storing the created Ansible server public key into the target db_server:
```sh
$ ssh-copy-id root@destinationIPaddress
```

If this does not work, it should be done manually, or dynamically during the instance creation.

## Check connections to hosts

Checking Ansible connection to remote hosts:
```sh
$ ansible all -m ping
```

Check your inventory: 
```sh
$ ansible-inventory –-list -y
```

Disable connection host key validation on the first connection (avoid to promt a yes input the first time you ssh to a host) by placing the following
```sh
[defaults]
host_key_checking = False
```
in a ansible.cfg file on the same folder as the playbook. To do it system-wide or for further info check this stackoverflow [question](https://stackoverflow.com/questions/32297456/how-to-ignore-ansible-ssh-authenticity-checking)

## Run a playbook

In the Ansible playbook folder, run:

```sh
$ ansible-playbook db-server-setup.yml
```
or
```sh
$ ansible-playbook /path/to/playbook.yml
```

## Playbook syntax fundamentals

A playbook is a file with `.yml` extension. These files contain instructions in [YAML syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html#yaml-syntax).
Playbooks are essentially a list of instructions to be performed sequentially in a specific destination host or group of hosts.

A playbook contains "plays", which are a set of tasks to be performed on some group of hosts. It can contain one or more plays, and each play with at least one task. The sintax is fairly intuitive:
```yaml
---
- name: update web servers
  hosts: webservers
  remote_user: root

  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
```
In the code above, the `-name` entry is the name of the play. It will be performed on the group "webservers" which must be declared in the `/etc/ansible/hosts` file. And the tasks will be run as `root`. 
Next there is the tasks section, where each task also begins with a `- name` entry. In the example depicted, we are using the [`yum` ansible module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html) to install the httpd package on a CentOS-like server. This would be equivalent to use the shell instruction ```sudo yum install httpd -y```, that in a playbook notation would be:
```yaml
---
- name: update web servers
  hosts: webservers
  remote_user: root
  
  tasks:
  - name: ensure apache is at the latest version
    shell:
      cmd: sudo yum install httpd -y
    register: yumInstallValue
    failed_when:
      - "'Error' in yumInstallValue.stderr"
```
Note how in the example above we are registering the shell output of the command in the variable `yumInstallValue`. We can refer to the stdout or stderr message with  `yumInstallValue.stdout` and `yumInstallValue.stderr`.

> In YAML indentation should be always with spaces and not tabs, and its crucial to respect indentation since a new block starts when a line with smaller indentation is found. 

A useful 

***

Return to **[main page](../README.md)** 
