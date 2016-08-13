
## Matplotlib

__Matplotlib__ can be used for creating plots and charts. The library is generally used as follows:

- Call a plotting function with some data (e.g. `.plot()`).
- Call many functions to setup the properties of the plot (e.g. `labels` and `colors`).
- Make the plot visible (e.g. `.show()`).


----


### Basic Line Plot

```python
import matplotlib.pyplot as plt import numpy
# create the data
myarray = numpy.array([1, 2, 3])
# plot the data
plt.plot(myarray)
# set the labels
plt.xlabel('some x axis')
plt.ylabel('some y axis')
# show the graph
plt.show()
```

Running the code above will output:

![](https://cl.ly/2o3B3R3n2t21/Image%202016-08-13%20at%201.19.50%20AM.png "line_plot")


----

### Basic Scatter Plot

```python
import matplotlib.pyplot as plt import numpy

x = numpy.array([1, 2, 3])
y = numpy.array([2, 4, 6])

plt.scatter(x,y)

plt.xlabel('some x axis')
plt.ylabel('some y axis')

plt.show()
```
