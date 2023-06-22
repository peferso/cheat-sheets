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

## Activating conda environments from bash script

If using miniconda

```sh
source ${HOME}/miniconda3/etc/profile.d/conda.sh
conda activate ${CONDAENV}
```

To check if the environment has been activated

```sh
echo "$(timestamp) activated conda environment: `echo $CONDA_DEFAULT_ENV`"
```

# Installing a package from code in a given folder

If we want to install a library from a code repository and refer to the code in that repository location

```sh
cd CODE_REPOSITORY_ROOT_FOLDER
pip install -e .
```

The modules from the repository can be imported in any other script using the same environment and the code will refer to the repository folder.

If working with an ipykernel (jupyter server), after any change is done to the repository code it will be necessary to restart the kernel for the changes to apply.

# Jupyter notebooks

Install jupyter notebook

```sh
pip install ipykernel notebook
```

## Jupyter server on a remote host

Run a jupyter server on a remote host:

```sh
jupyter notebook --no-browser --port=8080
```

It will promtp a token url.

To connect to the jupyter server, first create an ssh tunnel to the server:

```sh
ssh -L 8080:localhost:8080 $USER@$SERVER
```

Then paste the prompted url into your local browser.

## Include a python environment to be visible by the ipykernel

```sh
python -m ipykernel install --user --name=$CONDA_ENV
```

## Show RAM memory usage

To show the jupyter server memory usage on the notebook, first stop the jupyter server. Then:

```sh
pip install nbresuse
jupyter nbextension enable --py nbresuse --sys-prefix
```

And start the jupyter server.

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

# Numpy

```python
import numpy as np
```

## Arrays

```python
x = np.linspace(0, 5, 11) # array of 11 equally distance points ranging from 0 to 5
x = np.array(list(1,2,3,4,5)) # array from list
```

# Capturing Exceptions

Capture a specific sklearn warning:

```python
from sklearn.exceptions import ConvergenceWarning
import numpy as np
import warnings
try:
    np.seterr(all='warn')
    warnings.simplefilter("error", ConvergenceWarning)
    classifier.train()
except ConvergenceWarning as msg:
    # DO something
    raise Exception(msg)
```

# Argsparser: pass input parameters to script via command line

Using argsparse

```python
def init_argparse() -> argparse.ArgumentParser:
	...
    return parser


def main():
    logger.info('Script starts...')
    parser = init_argparse()
    args = parser.parse_args()
    if 'verbose' in dir(args):
        logger.setLevel(logging.DEBUG)
	par_1 = args.par_1
	...
```

Where input parameters can be defined in `init_argsparse()` as follows:

```python
def init_argparse() -> argparse.ArgumentParser:
    """Initialize the Argument Parser
	to handle the input arguments passed
    to the script.
    Returns:
        argparse.ArgumentParser: argument parser object
    """
    parser = argparse.ArgumentParser(
       usage='%(prog)s action -o [OPTION]',
       description=(
           'Some description that will be'
		   'printed to stdout if asked for'
		   'help.'
        )
    )
    parser.add_argument(
        'action',
        nargs='?',
        help='The action or task to run.',
        type=str,
        choices=[
           'one_positional_argument',
		   'another_positional_argument'
        ]
    )
    parser.add_argument(
        '--verbose',
        '-verbose',
        nargs='?',
        help=(
            'Show more logs info.'
        ),
        type=str,
        default=None
    )
    return parser
```

Control in `main` the presence of mandatory parameters:

```python
	...
	if args.action in ['one_positional_argument', 'another_positional_argument']:
		...
    else:
        parser.print_help()
        quit()
	...

```

# Ask for user input

```python
def ask_yes_or_no():
	answer = input("Continue? [enter y(yes) or n(no)]\n")
	entered_valid = False
	while not entered_valid:
		if answer.lower() in ["y"]:
			logger.info('Proceeding...')
			entered_valid = True
		elif answer.lower() in ["n"]:
			logger.info('Aborting...')
			quit()
		else:
			entered_valid = False
			print(f'Invalid input "{answer}"')
			answer = input("Continue? [enter y or n]\n")
```

# Using logger

Create a logger instance:

```python
import logging


def create_logger(
    name=__name__,
    level='INFO'
):
    logger = logging.getLogger(name)
    fh = logging.StreamHandler()
    fh_formatter = logging.Formatter(
      "[%(levelname)s][%(asctime)s][%(name)s:%(funcName)s] %(message)s"
    )
    fh.setFormatter(fh_formatter)
    if not logging.getLogger(name).hasHandlers():
        logger.addHandler(fh)
    lvl = getattr(logging, level)
    logger.setLevel(lvl)
    return logger
```

Then invoke from script or somewhere else:

```python
#Some script
...
logger = create_logger('test_cases_collector', level='INFO')
...

```

Change logging level

```python
logger.setLevel(logging.WARNING)
```

or by level name

```python
level = getattr(logging, level_name)
logger.setLevel(level)
```

# Parallelization

Use ray

```python
import ray
from tqdm import tqdm


def some_method(ncores):
	"""
	Here we parallelize
	"""
	...
	if ncores > 0:
		ray.init(
			num_cpus=ncores,
			log_to_driver=True
		)
		obj_ids = [
			compute_one_case_remote.remote(case) for case in cases_to_compute
		]
		_generation_output = [
			x for x in tqdm(to_iterator(obj_ids), total=len(obj_ids))
		]
		ray.shutdown()
	else:
        _generation_output = [
			compute_one_case(case) for case in tqdm(cases_to_compute)
		]
	for case, result in _generation_output:
		# retrieve the result of each case
		


def to_iterator(obj_ids):
    """
	Helper function that takes the parallelized ray entities and allows
    to track the progress of the reports parsing with tqdm.

    Parameters
    ----------
        obj_ids : ray.remote("input case") objects list

    Yields
    ------
        returns the result of each parallelized computation iteratively
    """
    while obj_ids:
        done, obj_ids = ray.wait(obj_ids)
        yield ray.get(done[0])


@ray.remote
def compute_one_case_remote(case):
	"""
	Ray wrapper function needed
	"""
    return compute_one_case(case)


def compute_one_case(case):
	"""
	Calculation to parallelize
	"""
	... # do something with input
    return case, result

```

***

Return to **[main page](../README.md)** 
