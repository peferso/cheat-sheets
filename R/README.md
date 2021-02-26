# R cheatsheet

## Basics

Transform data into code to make it reproducible to others
```R
dput(mtcars)
```
so that, in the script, you can paste the output of the previous command and assign it 
```R
df <- OUTPUT
```
The notation to refer to a specific _resource_ (function or dataframe) in a specific _package_ is:
```R
packagename::resource
```
Get help about a resource _foo_:
```R
?package::foo
?foo
```

## ggplot2 fundamentals

Scatter plot of _var1_ vs _var2_ data from dataframe _df_:
```R
ggplot(data = df) + 
  geom_point(mapping = aes(x = var2, y = var1))
```

Mapping a variable to an aesthetic:
```R
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
```
An aesthetic is a visual property of a point, i.e., shape, size, color, alpha (transparency), stroke, group, fill (see `?geom_point`).

To change aesthetics manually add it outside the ```aes()``` block:
```R
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), color = "green")
```

