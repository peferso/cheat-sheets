# Bash utilities (part 2): Bash programming

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

Check number of input arguments
```sh
if [ "$#" -ne 1 ]; then
    echo "Illegal number of parameters"
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

Compare two integers:

```sh
if (( a > b )); then
    ...
fi
# or
if [ "$a" -gt "$b" ]; then
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

Associative arrays, or arrays with keys-value pairs, example of dynamic creation from file:

```sh
declare -A REPOSITORIES
for rpt in `cat "${repositories_file}" | grep -v '#'`; do
	rpt_name=`echo ${rpt} | awk -F: '{print $1}'`
	rpt_flag=`echo ${rpt} | awk -F: '{print $2}'`
	AAT_REPOSITORIES+=(["${rpt_name}"]="${rpt_flag}")
done
```

Another example:

```sh
declare -a myArray

myArray[a]=123
myArray[b]=343
myArray+=([c]=345 [c]=054)
```

Access the keys:

```sh
for key in "${!array[@]}"; do
...
done

echo "${!array[@]}
```

Access the values:

```sh
echo ${myArray[a]}
echo ${myArray[@]}
```

## Functions

Example of declaring a function called "fun"
```sh
fun () {
 echo "Hello World! My name is $1 and I like $2"
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

Modify using patterns
```sh
string=str1_str2_str3.str4.str5
echo ${string##*_} # removes largest occurence of pattern at the begginning of string
echo ${string#*_} # removes smallest occurence of pattern at the begginning of string
echo ${string}
echo ${string%.*} # removes smallest occurence of pattern at the end of string
echo ${string%%.*} # removes largest occurence of pattern at the end of string
echo ${string/.*/repl01} # replaces first occurrence of pattern in string
echo ${string//.*/repl01} # replaces all occurrences of pattern in string
```

## File manipulation

Replace all "word" occurencies by "new" in all file "FILE"
```sh
sed -i -e "s/word/new/g" FILE
```

Remove lines from a file that contain a substring 
```sh
while read line; do
  [[ ${line} != *"RISK SCORE"* ]] && echo "$line" >> ${temp}
done < $outputData
```

## Date-Time stamp

Store current date-time stamp in a variable
```sh
dtimestamp=$( echo "$(date +'%Y%m%d-%H_%M_%S_%3N')" )
```

Add timestamp to echo's with a log format:
```sh
timestamp () {
    echo "[`date +'%Y-%m-%d %H:%M:%S'`]"
}

echo "$(timestamp) Some log message."
```

## Miscellaneous

Verify number of occurrences in a set of files:
```sh
files=(
   "./*.csv"
)

for file in ${files}
do
   I103=$(cat ${file} | grep -c '{2:I103')
   O103=$(cat ${file} | grep -c '{2:O103')
   nI103=$((I103+0))
   nO103=$((O103+0))
   ntotalMT103=$((nI103+nO103))
   total=$(cat ${file} | grep -c '\$')
   ntotal=$((total+0))
   if [ ${ntotal} -eq ${ntotalMT103} ] 
   then
      echo    
      echo ${file}
      echo '  'number of 103 occurrences in file: ${ntotalMT103}
      echo '  'total number of symbol occurrences in file: ${ntotal}
   fi
done
```

***

Return to **[bash - part 1](../bash/README.md)** 

Return to **[main page](../README.md)** 
