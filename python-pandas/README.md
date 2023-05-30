# Pandas

Import Pandas:
```python
import pandas as pd
```

# General settings

| Command | Description |
| ------- | ----------- |
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

# Others

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
