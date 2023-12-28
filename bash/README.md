# Bash utilities (part 1): System management

# Processes

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

Sort percentage of CPU's usage by user and process
```sh
ps -eo pcpu,pid,user,args | sort -r -k1 | less
```

A fancier top
```sh
htop
```

Sort processes by start time:
```sh
ps -ef --sort=start_time
```

Processes of a given user:
```sh
ps -fu username
ps -fu username --sort=start_time
```

# SystemD/systemctl

Check logs/Status of a process

```sh
journalctl -u httpd.service
```

Start a service

```sh
systemctl start a_service.service
```

Stop a service

```sh
systemctl stop a_service.service
```

# Cron and Crontab

Check system time:
```sh
timedatectl
```

Set local date time:
```sh
timedatectl set-timezone Europe/Madrid
```

Schedule a script run with a given user. As the user run

```sh
crontab -e
```

Example (run everyday a script at 13:55 h):

```sh
55 13 * * * /home/ec2-user/script_path/run-cleanhouse.sh
```

Check logs:

```sh
less /var/log/cron
```

# Logrotate

Look to which services use logrotate

```sh
/etc/logrotate.d/
```

Sometimes apache web server may be restarted/reloaded, check it in `/etc/logrotate.d/httpd`.

# Connectivity

Get the ip address:

```sh
hostname -I
```
Check if a remote host is accessible from the current one via a specific port number:
```sh
exec 3<> /dev/tcp/180.18.153.59/7222
```
if there is not a *[Connection timed out]*, then it is accessible. Output can be checked with ```echo $?```.

# File management

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

Display json contents pretty:

```sh
pyhon -m json.tool json_file.json
```

# Links

Create a symbolic link 

```sh
ln -s {source-filename} {symbolic-filename}
```

# Tarballs

Create a tarball file without compression of a folder in a different folder (note the ordering, destination goes first!)

```sh
tar -cvf PATH_TO_DESTINATION_FILE.tar SOURCE_FOLDER_1 SOURCE_FOLDER_2 ... SOURCE_FILE_1 SOURCE_FILE_2 ...
```

Use compression

```sh
tar -cvzf PATH_TO_DESTINATION_FILE.tar.gz  SOURCE_FOLDER_1 SOURCE_FOLDER_2 ... SOURCE_FILE_1 SOURCE_FILE_2 ...
```

List the contents of a tar file

```sh
tar -ztvf TARFILE
```

Unroll a tarball specifying the destination folder

```sh
tar -xvf TARFILE -C DESTINATION_FOLDER
```

# ssh

Ssh using an existing private key .pem file
```sh
ssh -i "path_to_pem_file" user@dns
```

Generate a key pair... [TODO]

Generate a public key from an already existing private key:
```sh
ssh-keygen -y -f id_rsa > id_rsa.pub
```

Migrate an already existing key:
```sh
cd .ssh
touch id_rsa
vi id_rsa # paste content of private key .pem file and save
chown user:group id_rsa # the user authenticating must be the owner of the key
chmod 600 id_rsa # permissions must be read/write only for the key owner
```

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

# Environment variables

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

# Groups and users

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

# Adding a directory to the PATH variable

```sh
export PATH=new_path:$PATH
```

To see which directory is used for a program 
```sh
which python
```

# Aliases

Define alias

```sh
alias aliasname='some command'
```

A useful method is to gather aliases in files `~/.bash_aliases`, then load them during profile adding to `~/.profile` or `~/.bashrc` the following 

```sh
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

## awk inside alias

Aliases are declared between single quotes.

When using awk, the print part goes also between single quotes:

```sh
cat file.txt | grep 'log-2' | awk -F ; '{print $2}'
```

To use the awk command above in an alias we should write single quotes `'` as `'\''`:

```sh
alias an_awk_command='cat file.txt | grep '\''log-2'\'' | awk -F ; '\''{print $2}'\'''
```

***

Continue to **[bash - part 2](../bash-2/README.md)** 

Return to **[main page](../README.md)** 
