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
plot with different colors depending on condition:
```R
 ggplot(data = mpg) + 
      geom_point(mapping = aes(x = displ, y = hwy, colour = displ < 5))
```

Facet a plot separating data in different plots grouped by a third discrete or cathegorical variable value:
```R
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
```
above, `~ variable` is a data structure called a _formula_ in R. Also, facet the plot with two discrete variables:
```R
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(drv ~ class)
```
Use different `geom` objects to represent data: geom_points, geom_smooth...
You can overlap different _geom_'s of the same data using the mapping argument:
```R
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
```
mappings declared this way are **global**, any further mapping introduced inside any _geom_ block will be a **local** one, and will replace settings of the global mapping.
```R
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth()
```
