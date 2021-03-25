# Bash utilities (part 1): Bash programming

## Redirect stderr and stdout to trash

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

## If statement

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



## Arrays

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
also, to loop over integer values:
```sh 
for i in {1..32}
do
 ...
done
```
or in one line
```sh
for i in {1..90}; do; echo "" > file-$i.txt ; done
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
## Functions

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

## Split a string using a delimiter

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

## Store stdout in a variable

Examples
```sh
var1=$(echo ls -lrt)
var2=$(cat file.log)
```

## String manipulation

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

## File manipulation

Replace all "word" occurencies by "new" in all file "FILE"
```sh
sed -i -e "s/word/new/g" FILE
```

## Date-Time stamp

Store current date-time stamp in a variable
```sh
dtimestamp=$( echo "$(date +'%Y%m%d-%H_%M_%S_%3N')" )
```

***

Return to **[bash - part 1](../bash/README.md)** 
Return to **[main page](../README.md)** 
