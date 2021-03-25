# Bash utilities (part 1): System management

## Processes

Print out the running processes:

```sh
ps -ef 
```

Also, we can use the top command
```sh
top
```
With top running, we can type `h` to open the help menu.
We can track processes triggered by a specific user calling top as follows:
```sh
top -u username
```
For example, typing `V` we can toggle the view to view a forest tree process hierarchy.

## Connectivity

Get the ip address:

```sh
hostname -I
```
Check if a remote host is accessible from the current one via a specific port number:
```sh
exec 3<> /dev/tcp/180.18.153.59/7222
```
if there is not a *[Connection timed out]*, then it is accessible. Output can be checked with ```echo $?```.

## File management

Find all lines that contain a pattern 

```sh
grep -option pattern file
```

Option can be ```i``` if you want to ignore uppercase characters.

Option can be `A N` if you want to retrieve the `N` lines **after** each match

Option can be `B N` if you want to retrieve the `N` lines **before** each match

Option can be `C N` if you want to retrieve the `N` lines **around** each match

File can be ```*``` for all files in the working directory.

File can be ```-r``` to search recursively in all files and directories contained in the working directory.

Find all files with a given name pattern inside a folder
```sh
find /folder -type f -name '*.html'
```

By exact name
```sh
find -type f -name exact-name.ext
```



## ssh

Ssh using an existing private key .pem file
```sh
ssh -i "path_to_pem_file" user@dns
```

Generate a key pair... [TODO]

For a specific user, the key pairs are stored in the corresponding home under ```.ssh```, e.g.,
```sh
/home/ec2-user/.ssh
/var/lib/jenkins/.ssh
```
on ```.ssh``` the ```id_rsa``` is the private key and ```id_rsa.pub``` is the public key. 
The public key file contains one line which is the public key indeed.
Note that any possible destination host must have this public key included in its ```authorized_keys``` file, also inside ```.ssh```.

If you bring an already existing key to a new user, you should create the ```id*``` files as the user and copy the keys. Finally, they need to have 600 permissions:
```sh
sudo chmod 600 $privateKeyFile
sudo chmod 600 $publicKeyFile
```

## Environment variables

Create a new one using the terminal
```sh
export NEWVAR="something"
```

Delete an existing environment variable:
```sh
unset NEWVAR
```

You can also add the variables permanently to the environment
```sh
echo 'NEWVAR="content/asdf"' >> /etc/environment
echo 'PUBLIC_IP='$PUBLIC_IP >> /etc/environment
```

## Groups and users

Create a group
```sh
sudo groupadd jenkinsGroup
```

Add an existing user to a group
```sh
sudo usermod -a -G ec2-user jenkins
```

Display all users
```sh
cat /etc/passwd
```

Display user groups
```sh
id ec2-user
```

Create a new user and assign groups in a single command
```sh
sudo useradd -g primary_group -G sec_groupA,sec_groupB new_user
```

Change to a user of a service account (without shell by design), such as jenkins user. It is necessary to specify the shell explicitly in these cases:
```sh
sudo su -s /bin/bash jenkins
```

## Adding a directory to the PATH variable

```sh
export PATH=new_path:$PATH
```

To see which directory is used for a program 
```sh
which python
```

***

Continue to **[bash - part 2](../bash-2/README.md)** 

Return to **[main page](../README.md)** 
