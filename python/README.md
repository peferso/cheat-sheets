# python basics

Packages are easily managed using conda or the anaconda framework.

Print python version
```python
import sys
print('Python version', sys.version)
```

Print packages version:
```python
import requests
import json
print('requests:', requests.__version__)
print('json:', json.__version__)
```
> Not all the packages have a `__version__` value.

# Install environment into folder project

First "cd" to project folder and then run

```sh
virtualenv -p python3 .
```
It will install the python environment in the project folder. 

To activate the environment:

```sh
source project_folder/bin/activate
```
Once the environment is activated we can install the required packages with e.g.
```sh
pip install pandas
```
To list the packages installed in the activated environment:
```sh
pip freeze
```
The python code can be put into ```project_folder/src```.

# Anaconda

Anaconda is a software that provides a python package and environment manager, together with other tools such as Jupyter Notebooks, JupyteLab... 
Check out this [cheatsheet](file:///C:/Users/n522286/Downloads/conda-cheatsheet.pdf). 

It is very useful to isolate python environments and install different packages and versions.

Start conda
```sh
conda activate
```

List existing environments:
```sh
conda info --envs
```

Create a new environment with specific python version
```sh
conda create -n envname python=3.8
```


# Basic syntax: lists

Python list indexing might be a bit funny compared with other frameworks. If we have this list:
```python
myList = ['a', 1, 'b', 'c', 4]
```
Then the elements can be accessed:
```python
myList[0:]
myList[1:]
myList[2:]
myList[3:]
myList[4:]
```
The last element is `myList[-1:]`, and the rest can be also accessed in reverse order: `myList[-2:]`, `myList[-3:]`...
To get slices then we must be careful. The notation is:
```python
myList[a:b]
```
with `a`, `b` integer numbers.
* `a`: is the position in the list where we start selecting elements, a-th element is included.
* `b`: is the number of elements of the slice, in a way that the elements retrieved will be those with indexes values of $$a, a+1, a+2, ..., a+b-1 $$ 
Lists can be created as follows:
```python
x = [ i for i in range(4) ]
[item**2 for item in x]
```
## Printing

Printing strings passing variables as arguments:
```python
a = 'bla bla'
b = 3.46
print('A thing: {one}, and another thing: {two}'.format(one=a,two=b))
print('A thing: {}, and another thing: {}'.format(a,b))
``` 

## The range function

The syntax of the range function is `range(a,b,step)`, it generates a list of integers where
* `a`: is the first integer 
* `b+a`: is the last integer, if consistent with the `step` value.
* `step`: is the value of the increment used to generate the list of numbers, the difference between consecutive elements. 

## Lambda expressions, map and filter

Lambda expressions are shorthands for function definitions. For example, if you want to operate over some float it is possible to declare a function or do it without a function declaration with a lambda expression:
```python
lambda x: x**(3/1.5) + x**8
```
The above declaration is equivalent to 
```python
def fun(x):
	return x**(3/1.5) + x**8
```
We can apply such operation to a list of inputs with the help of map function:
```python
values = [1, 32, 11, 221, 134]
list(map(fun, values))
list(map(lambda x: x**(3/1.5) + x**8, values))
```
With the filter method, we can filter out elements satisfying a condition, for example
```python
elements = [1,2,3,4,5,6,7,8]
list(filter(lambda x: x%2 != 0, elements))
``` 
The above filter uses a lambda function to evaluate each value in the list 'elements' checking if its module over 2 is different from zero, and keeps all elements that satisfy such condition (the odd numbers).

# Pandas

Import Pandas:
```python
import pandas as pd
```


## Count number of rows and columns

The number of rows in a dataframe `df` is given by:
```python
numRows = df.shape[0]
print(numRows)
```

The number of columns:
```python
numCols = df.shape[1]
print(numCols)
```

## Managing excel files

Import an excel file as a pandas dataframe:
```python
import pandas as pd
path = './path_to_file'
df = pd.read_excel(path)
```

Export an existing dataframe `df` to an excel file:
```python
export_excel = df.to_excel(path_to_exported_file, index=None, header=True)
```

## Loop over dataframe rows

Given an existing dataframe `df`, we can loop over the rows and access specific columns as follows:
```python
for index, row in df.iterrows():
    CUSTOMER_NAME = str(row['CUSTOMER NAME'])
    DATE_OF_BIRTH = str(row['DATE OF BIRTH'])
    ...
```

## Loop over dataframe rows and columns

Given an existing dataframe 
```python
for column in df.columns:
	for index, row in df.iterrows():
		df.loc[index, column] = row[column].replace("foo", "bar")
```

## Filter dataframe rows by a condition

Given an existing dataframe `df` and a condition, the following code filters the rows for which the condition is true:
```python
is_it_satisfied = df["Recommendation based on Requirement"] == "yes"
df_requiredRows = df[is_it_satisfied]
```

## Add a column to a dataframe

```python
...
    l_years = []
    for index, row in df.iterrows():
        years = row['batch_ts'].year - int(row['year'])
        l_years.append(years)
    df['years'] = l_years
...
```

## Drop a column from a dataframe

```python
    df = df.drop('year', axis=1)
```

## Apply operations to dataframe rows
Filter all rows which column contains a word or some text:
```python
df[df['col_name'].apply(lambda x: True if 'word' in x.lower().split(' ') else False)]

df[df['col_name'].apply(lambda x: True if 'substring' in x.lower() else False)]
```

## Group by
```python
df.groupby('JobTitle')['Id'].count().sort_values(ascending=False)[0:5]
```

## Correlation of two columns
```python
df[['col_a', 'col_b']].corr()
```


***

Return to **[main page](../README.md)** 
