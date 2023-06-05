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

***

Return to **[main page](../README.md)** 


