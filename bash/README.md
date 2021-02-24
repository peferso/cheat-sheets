# Bash utilities

## System management

### Processes

Print out the running processes:

```sh
ps -ef 
```

### Connectivity

Get the ip address:

```sh
hostname -I
```
Check if a remote host is accessible from the current one via a specific port number:
```sh
exec 3<> /dev/tcp/180.18.153.59/7222
```
if there is not a *[Connection timed out]*, then it is accessible. Output can be checked with ```echo $?```.

### File management

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



### ssh

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

### Environment variables

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

### Groups and users

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


## Bash programming

### Redirect stderr and stdout to trash

Only stderr
```sh
2>/dev/null  
```

Only stdout
```sh
1>/dev/null
```

Both stderr and stdout
```sh
2> /dev/null 1>&2
```

### If statement

Check if input is empty
```sh
if [ -z $1 ]
then
 ...
fi
```

Check if a file exists:
```sh
exists=$( 2>/dev/null ls ${MAINDIR}/${TFFILE} )
if [ -z "${exists}" ]
then
  echo "File ${MAINDIR}/${TFFILE} does not exists"
else
 ...
fi
```



### Arrays

Declare arrays
```sh
x=();
y=("anr" "123" "asd");
```

Access to specific elements
```sh
echo "${y[0]}" "${y[2]}" "${y[-1]}" "${y[-2]}"
```

Access to all the elements
```sh
echo "${y[@]}"
```

Loop over all the elements
```sh 
for i in "${y[@]}"
do
 ...
done
```

Append an element to existing array
```sh
array+=new_element
```

Create a new array copying an existing one
```sh
newArray=( "${oldArray[@]}" )
```

Print the number of elements of an array
```sh
echo "${#array[@]}"
```

An 
### Functions

Example of declaring a function called "fun"
```sh
fun () {
 echo "Hello World! My name is $0 and I like $1"
}
```
so that, we invoke it as follows:
```sh
fun "Peter" "bash programming"
```

### Split a string using a delimiter

Function that splits a string and stores elements splitted in an array
```sh
func_splitter() {
  str=$1
  delimiter=$2
  s=$str$delimiter
  array=();
  while [[ $s ]]; do
      array+=( "${s%%"$delimiter"*}" );
      s=${s#*"$delimiter"};
  done;  
}
```

Call it to split a ```string``` using a specific delimiter, like ```-```, as follows
```sh
func_splitter "$string" "$delimiter"
echo "${array[@]}"
```

Simply split a string in two parts 
```sh
partLeft=${string%%"$delimiter"*}
partRight=${string#*"$delimiter"}
```

### Store stdout in a variable

Examples
```sh
var1=$(echo ls -lrt)
var2=$(cat file.log)
```

### String manipulation

Check if a string contains only alphanumeric and {"```-```", "```_```"} characters.
```sh
if [[ "${ENVNAME}" =~ ^[a-zA-Z0-9_-]+$ ]]
then
  echo "${ENVNAME}"
else
  echo "${ERRMSSGA}"
  exit
fi
```

Read a string line by line:
```sh
#!/bin/bash
while IFS= read -r line
do
  echo "$line"
done <<< "${string}"
```

Read a file line by line:
```sh
#!/bin/bash
while IFS= read -r line
do
  echo "$line"
done < "${pathToFile}"
```

Replace a string substring by a new one
```sh
string="a new new test"
substring="new"
newone="old"
echo "${string/$substring/$newone}" 
```

Replace all occurrences in a string of a substring by a new one
```sh
message='The secret key is 431241'
echo "${message//[0-9]/X}"           
# prints 'The secret key is XXXXX'
```

Remove white spaces
```sh
var=$(echo "${var}//[:blank:]]/")
```

Remove all control characters:
```sh
var=${var//[ $'\001'-$'\037']}
```` 

Transform string to integer *after removing control characters*
```sh
expr $string
num1=$((string + 0))
num2=$((stra + strb))
```

### File manipulation

Replace all "word" occurencies by "new" in all file "FILE"
```sh
sed -i -e "s/word/new/g" FILE
```

### Date-Time stamp

Store current date-time stamp in a variable
```sh
dtimestamp=$( echo "$(date +'%Y%m%d-%H_%M_%S_%3N')" )
```

***

Return to **[main page](../README.md)** 
