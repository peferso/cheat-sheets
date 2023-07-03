# Pandas - 1

Import Pandas:
```python
import pandas as pd
```

# General settings

| Command | Description |
| ------- | ----------- |
| `pd.set_option('display.max_columns', 1000)`  | Increase the number of columns displayed |
| `pd.set_option('display.max_rows', 1000)`     | Increase the number of rows displayed |
| `pd.set_option('display.max_colwidth', None)` | Increase the cell size limit |

# Date operations

First, transform the column to datetime format specifying the 
string format to be parsed:

```python
df[col] = pd.to_datetime(df[col], format='%Y-%m-%d', errors=_errors)
```

By setting `_errors` to `raise` (default), inconsistent dates will
raise exceptions. To ignore them and set as missing use `coerce`.

## Add days to a date column

```python
df[col] = df[col] + pd.Timedelta(ndays, unit='D')
```

## Add months to a date

```python
pd.to_datetime(a_date_string, format='%Y-%m-%d') + pd.DateOffset(months=nmonths)
```

## Get the time difference among two dates

```python
(t2 - t1).days
```

## Remove timezone

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


***

Continue to **[Python-pandas (2)](README-2.md)** 

Return to **[main page](../README.md)** 
