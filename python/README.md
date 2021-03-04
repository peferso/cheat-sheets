# python basics

## Anaconda

Anaconda is a software that provides a python package and environment manager, together with other tools such as Jupyter Notebooks, JupyteLab... 
Check out this [cheatsheet](file:///C:/Users/n522286/Downloads/conda-cheatsheet.pdf). 

It is very useful to isolate python environments and install different packages and versions.

## Basic syntax: lists

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

### The range function

The syntax of the range function is `range(a,b,step)`, it generates a list of integers where
* `a`: is the first integer 
* `b+a`: is the last integer, if consistent with the `step` value.
* `step`: is the value of the increment used to generate the list of numbers, the difference between consecutive elements. 

***

Return to **[main page](../README.md)** 
