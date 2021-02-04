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
in a ansible.cfg file on the same folder as the playbook. To do it system-wide or for further info check this stackoverflow [question]:<https://stackoverflow.com/questions/32297456/how-to-ignore-ansible-ssh-authenticity-checking>

## Run a playbook

In the Ansible playbook folder, run:

```sh
$ ansible-playbook db-server-setup.yml
```
or
```sh
$ ansible-playbook /path/to/playbook.yml
```
