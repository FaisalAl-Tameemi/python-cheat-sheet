
# Python Cheat Sheet

## Modules

To import specific parts of a module.

```python
from collections import defaultdict, Counter
lookup = defaultdict(int)
 my_counter = Counter()
```

You can also rename the module that you're importing within the file.

```python
import matplotlib.pyplot as plt
```

**Careful:**

```python
match = 10 
from re import * # uh oh, re has a match function
print match # "<function re.match>"
```


----


## Functions

Let's create a function that returns the double of a given number.

```python
def double(x): 
	"""this is where you put an optional docstring that
		 explains what the function does. for example,
		 this function multiplies its input by 2"""
	return x * 2
```

Now we can pass that function to other functions

```python
def apply_to_one(fn):
	return fn(1)

my_double = double
x = apply_to_one(my_double)
```

or you can use annonymous functions:

```python
y = apply_to_one(lambda x: x + 4)
```

You can also set a lambda to a variable but it's **not recommended**:

```python
another_double = lambda x: 2 * x # don't do this
def another_double(x): return 2 * x # do this instead
```

### Default Values in Parameters

```python
def my_print(message="my default message"):
	print message

my_print("hello") # prints 'hello'
my_print() # prints 'my default message'
```

### Specify Arguments by Name

```python
def subtract(a=0, b=0):
	return a - b

subtract(10, 5) # returns 5
subtract(0, 5) # returns -5 subtract(b=5) # same as previous
```


----


## Exceptions

```python
try:
	print 0/0
except ZeroDivisionError:
	print "cannot divide by zero"
```

----


## Lists

Lists is Python are very important. They're similar to arrays in other languages with some extended functionality:

```python
integer_list = [1, 2, 3]
heterogeneous_list = ["string", 0.1, True]
list_of_lists = [ integer_list, heterogeneous_list, [] ]

list_length = len(integer_list) # equals 3
list_sum = sum(integer_list) # equals 6
```

### Getting and Setting List Items

```python
x = range(10) # is the list [0, 1, ..., 9]
zero = x[0] # equals 0, lists are 0-indexed
one = x[1]
nine = x[-1] # equals 9, 'Pythonic' for last element
eight = x[-2] # equals 8, 'Pythonic' for next-to-last element
x[0] = -1 # now x is [-1, 1, 2, 3, ..., 9]
```

### Slicing Lists

```python
first_three   = x[:3]
three_to_end  = x[3:]
one_to_four   = x[1:5]
last_three    = x[-3:]
copy_of_x     = x[:]
without_first_and_last = x[1:-1]
```


### Check if An Element Is In List


```python
1 in [1, 2, 3] # True
0 in [1, 2, 3] # False
```


### Concatenating Lists


```python
x = [1, 2, 3]
x.extend([4, 5, 6]) # x is now [1,2,3,4,5,6]
```

The example below doesn't modify the variable (i.e. `y`)

```python
x = [1,2,3]
y = x + [4, 5, 6] # y is [1, 2, 3, 4, 5, 6]; x is unchanged
```

More frequently you'll be using `append` to add one element at a time:

```python
x = [1,2,3]
x.append(0) # x is now [1, 2, 3, 0]
y = x[-1] # equals 0
z = len(x) # equals 4
```

You can unpack lists as follows:

```python
x, y = [1, 2] # now x is 1, y is 2
_, y = [1, 2] # now y == 2, didn't care about the first element
```

----

## Tuples

Tuples are lists’ immutable cousins. Pretty much anything you can do to a list that doesn’t involve modifying it, you can do to a tuple.

```python
my_list = [1, 2]
my_tuple = (1, 2)
other_tuple = 3, 4
my_list[1] = 3 # my_list is now [1, 3]

try:
	my_tuple[1] = 3
except TypeError:
	print "cannot modify a tuple"
```

It's a great way to get multiple values out of a function:

```python
def sum_and_product(x, y):
	return (x + y),(x * y)

sp = sum_and_product(2, 3) # equals (5, 6)
s, p = sum_and_product(5, 10) # s is 15, p is 50
```

----


## Dictionaries


To create a dict in python, you can do the following:

```python
empty_dict = {} # Pythonic
empty_dict2 = dict() # less Pythonic
grades = { "Joel" : 80, "Tim" : 95 } # dictionary literal
print joels_grade = grades["Joel"] # => prints 80
```

You'll get a `KeyError` if the key isn't available:

```python
try:
	kates_grade = grades["Kate"]
except KeyError:
	print "no grade for Kate!"
```

To check if a certain key exists:

```python
joel_has_grade = "Joel" in grades # True
kate_has_grade = "Kate" in grades # False
```

**Return a default value if key not found:**

```python
joels_grade = grades.get("Joel", 0) # equals 80
kates_grade = grades.get("Kate", 0) # equals 0
no_ones_grade = grades.get("No One") # default default is None
```

To assign a key and a value pair:

```python
grades["Tim"] = 99 # replaces the old value
grades["Kate"] = 100 # adds a third entry
num_students = len(grades) # equals 3
```

### Working w. Lists

Other than looking up values and setting them, dicts have the following functions:

```python
tweet_keys = tweet.keys() # list of keys
tweet_values = tweet.values() # list of values
tweet_items = tweet.items() # list of (key, value) tuples

"user" in tweet_keys # True, but uses a slow list in
"user" in tweet # more Pythonic, uses faster dict in
"joelgrus" in tweet_values # True
```

### Working with `defaultdict`s

The example below is a **Word Counter** which **doesn't** use `defaultdict`:

_Note: In this case you have to check every time if the key exists._

```python
word_counts = {}
for word in document:
	try:
		word_counts[word] += 1
	except KeyError:
		word_counts[word] = 1
```

The example below is a **Word Counter** which also **doesn't** use `defaultdict` but uses the `get` method:

```python
word_counts = {}
for word in document:
	previous_count = word_counts.get(word, 0)
	word_counts[word] = previous_count + 1
```

Now with `defaultdict`:

**Note:** _A defaultdict is like a regular dictionary, except that when you try to look up a key it doesn’t contain, it first adds a value for it using a zero-argument function you pro‐ vided when you created it. In order to use defaultdicts, you have to import them from collections._

```python
from collections import defaultdict

word_counts = defaultdict(int) # int() produces 0
for word in document:
	word_counts[word] += 1
```

Some more examples:

```python
dd_list = defaultdict(list) # list() produces an empty list
dd_list[2].append(1) # now dd_list contains {2: [1]}

dd_dict = defaultdict(dict) # dict() produces an empty dict
dd_dict["Joel"]["City"] = "Seattle" # { "Joel" : { "City" : Seattle"}}

dd_pair = defaultdict(lambda: [0, 0])
dd_pair[2][1] = 1 # now dd_pair contains {2: [0,1]}
```

----


## Counter


A Counter turns a sequence of values into a defaultdict(int) like object mapping keys to counts.

**Widely used to create histograms.**


----


## Sets


Another data structure is set, which represents a collection of distinct elements:


```python
s = set() # s is now { 1 }
s.add(1) # s is now { 1, 2 }
s.add(2) # s is still { 1, 2 }
s.add(2) # equals 2
x = len(s)
y = 2 in s # equals True
z = 3 in s # equals False
```

----


## Control Flow


```python
if 1 > 2:
	message = "if only 1 were greater than two..."
elif 1 > 3:
	message = "elif stands for 'else if'"
else:
	message = "when all else fails use else (if you want to)"
```

Or with ternary

```python
parity = "even" if x % 2 == 0 else "odd"
```

Or `continue` and `break`:

```python
for x in range(10):
	if x == 3:
		continue # go immediately to the next iteration
	if x == 5:
break # quit the loop entirely print x
```

----


In python, `nil` or `null` is denoted as `None`.


----


Let's say we wanted to read a value based on a condition:

```python
s = some_function_that_returns_a_string()
if s:
	first_char = s[0]
else:
	first_char = ""
```

A simpler way to do this is as follows:

```python
first_char = s and s[0]
```

```python
safe_x = x or 0
```


----
