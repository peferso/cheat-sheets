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

File can be ```*``` for all files in the working directory.

File can be ```-r``` to search recursively in all files and directories contained in the working directory.

### ssh

Ssh using an existing private key .pem file
```sh
ssh -i "path_to_pem_file" user@dns
```

Generate a key pair

## Bash programming

### Redirect stderr and stdout to trash

Only stderr
```sh
2>/dev/null  

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
### Functions

Example of declaring a function called "fun"
```sh
fun () {
 echo "Hello World! My name is $0 and I like $2"
}
```
so that, we invoke it as follows:
```sh
fun "Peter" "bash programming"
```

### Split a string using a delimiter

Function that splits a string and stores elements splitted into an array
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

### Store stdout into a variable

Examples
```sh
var1=$(echo ls -lrt)
var2=$(cat file.log)
```
