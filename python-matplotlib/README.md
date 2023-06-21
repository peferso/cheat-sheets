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

***

Return to **[main page](../README.md)** 


