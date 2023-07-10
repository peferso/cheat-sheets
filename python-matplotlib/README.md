# Matplotlib

```python
import matplotlib.pyplot as plt
```

# Duplicate y axis (mirror)

```python
ax2 = ax.twinx()
```

# Remove axis info

## All info

```python
axes.get_xaxis().set_visible(False)
axes.get_yaxis().set_visible(False)
```

## Only tick labels

```python
ax.get_xaxis().set_ticklabels([])
```

# Iterate over colors 

 ```python
colors = iter(plt.cm.rainbow(np.linspace(0, 1, delta)))
 ```

# Change font sizes

Accessing the axis instance:

```python
for item in (
    [
        axis.title,
        axis.xaxis.label,
        axis.yaxis.label
    ] +
    axis.get_xticklabels() +
    axis.get_yticklabels()
):
    item.set_fontsize(font_size)  
```

The fontsize of legend of the plot:

```python
import matplotlib.font_manager as font_manager


legend_prop = font_manager.FontProperties(size=font_size)
plt.legend(prop=legend_prop, loc='upper left')
```

# multiple plots
```python
plt.subplot(n, m, 1) # declare a grid of plots with n rows and m columns and choose the 1 one
plt.plot(x,y,...) # plot
...
plt.subplot(n, m, 2) # choose the next item in the plots grid
plt.plot(x,y,...)
...
```

# Object oriented way

A plot
```python
fig = plt.figure() # create canvas, figure object

axes = fig.add_axes([0.1,0.1,0.8,0.8]) # left position coordinates and width and height

axes.plot(x,y)
axes.set_xlabel(string1)
axes.set_ylabel(string2)
axes.set_title(title)
```

A plot inside a plot
```python
fig = plt.figure() # create canvas, figure object

axes1 = fig.add_axes([0.1,0.1,0.8,0.8])
axes2 = fig.add_axes([0.2,0.5,0.4,0.5])
```

Subplots
```python
fig, axes = plt.subplots(nrows=n, ncols=m) # devuelve una lista de objetos "axes"

axes[0].plot(x,y)
axes[0].set_title('first plot')

axes[1].plot(y,x)
axes[1].set_title('second plot')

plt.tight_layout() # prevents overlapping
```

Legends
```python
fig = plt.figure()

ax = fig.add_axes([0,0,1,1])

ax.plot(x,x**2, label = 'X squared')
ax.plot(x,x**3, label = 'X cubed')

ax.legend(loc=0) # use location code, check documentation

plt.show()
```

Plot style
```python
...
ax.plot(x ,y, 
	color='#RGB_Hex_Code', 
	lw=2 # multiplies the default with of the line , 
	alpha=0.5 # transparency, 
	ls='code', 
	marker='code',
	markersize=scale,
	markerfacecolor=color,
	markeredgewidth=scale
	markeredgecolor=color)
	
ax.set_xlim([x1,x2])# bounds of x axis
ax.set_ylim([y1,y2])# bounds of y axis
```


Figure size and DPI (dots per inch)
```python
fig = plt.figure(figsize=(3,2), dpi=100)
```

Save a figure to a file
```python
fig.savefig('my_picture.png', dpi = 200) # extension applied is png, but we can use others such as jpeg...
```

# Secondary or twin axis

```python
fg, ax = plt.subplots(1, 1, figsize=(1.618*6, 6))

funded_data.groupby(['date']).size().plot.bar(ax=ax, label='volume')

ax2 = ax.twinx()
funded_data.groupby(['date'])['FLAG'].mean().plot(ax=ax2, color='black', linewidth=2.0, label='Flag rate')

ax.legend()
ax2.legend()
```

# OLS fit plot

Once we've made the OLS fit:

```python
import statsmodels.api as sm


y = grouped_by_dec['percentage'].values
X = grouped_by_dec['decile'].values
X = sm.add_constant(X)
model = sm.OLS(y, X)
result = model.fit()
```

We can plot the data points and the regression line as follow:

```python
fig = plt.figure()
ax = fig.add_axes([0.1,0.1,0.6*1.618,0.6])
ax.scatter(X[:, 1], y, s=10)
y_pred = result.predict(X)
ax.plot(X[:, 1], y_pred, color='gray', linestyle='-')
ax.set_xlabel('Decile')
ax.set_ylabel('Percentage [%]')
ax.text(
	0.825,
	0.925,
	(
		f'$R^2$={result.rsquared:.3f}\n'
		r'$\rho_{Pearson}$'f'={correlation:.3f}'
	),
	horizontalalignment='left',
	verticalalignment='center',
	transform=ax.transAxes
)
ax.set_title(f'Month: {month}')
plt.show()
```

# Seaborn

```python
import seaborn as sns
%matplotlib inline
```

***

Return to **[main page](../README.md)** 


