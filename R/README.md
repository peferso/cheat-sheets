# Introduction

This notes contain several commands useful to use RStudio.
I find mandatory to follow this [online book](https://r4ds.had.co.nz/index.html) by Hadley Wickham and Garret Grolemund. 

# Basics

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

Press ALT+SHIFT+K to access the shortcuts.

## ggplot2 fundamentals

Check out this [awesome ggplot2 pdf cheatsheet](https://rstudio.com/wp-content/uploads/2016/11/ggplot2-cheatsheet-2.1.pdf).


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

## Console

If you want to assign and print the assignment in a single line use `()` around the assignment:
```R
(y <- seq(1, 10))
```

## dplyr basics

Data can be transformed using the basic dplyr functions, whose syntax is:
```R
new_dataframe <- verb(old_dataframe, what to do options)
```

Filter:
```R
jan1 <- filter(flights, month == 1, day == 1)
```
comparison operators are **>**, **>=**, **<**, **<=**, **!=** (not equal), and **==** (equal). Conditions can be combined using  **&** “and”, **|** “or”, and **!** “not”.
When comparing real numbers it might be suitable using `near(a, b)` instead of `a == b` to avoid floating precision errors. 
There is the option **x %in% y** that will filter rows where _x_ is found in _y_
```R
nov_dec <- filter(flights, month %in% c(11, 12))
```
Ordering rows is achieved with `arrange`. Values with NA are shown at the end, they can be printed first as follows:
```R
arrange(data_frame, desc(is.na(column_name)))
```
Selecting columns by variable name and dropping the rest
```R
select(flights, year, month, day)
select(flights, year:day)
select(flights, -(year:day))
```
variables can be identified with auxiliary functions as _starts_with_, _end_with_, see ?select.
Change the order of some variables: 
```R
select(flights, time_hour, air_time, everything())
```
Create new variables from existing ones with `mutate`:
```R
# Create a new dataframe from existing one
flights_sml <- select(flights, 
  year:day, 
  ends_with("delay"), 
  distance, 
  air_time
)
# Append the new columns
mutate(flights_sml,
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```
Or, if you want to replace all variables by the new ones:
```R
transmute(flights,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```
Using `summary` together with `group_by` to obtain statistics group by group:
```R
by_day <- group_by(flights, year, month, day)
summarise(by_day, delay = mean(dep_delay, na.rm = TRUE))
```
The same code can be written with pipes: `%>%`. Pipes such as `x %>% f(y) %>% g(z)` perform `g(f(x, y),x)`.
```R
flights %>% 
  group_by(year, month, day) %>% 
  summarise(mean = mean(dep_delay, na.rm = TRUE))
```

***

Return to **[main page](../README.md)**
