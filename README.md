
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


## Sorting


```python
x = [4,1,2,3]
y = sorted(x) # is [1,2,3,4], x is unchanged
x.sort() # now x is [1,2,3,4]
```

To sort lists in custom ways:

```python
# sort the list by absolute value from largest to smallest
x = sorted([-4,1,-2,3], key=abs, reverse=True) # is [-4,3,-2,1]

# sort the words and counts from highest count to lowest
wc = sorted(word_counts.items(),
					 key=lambda (word, count): count,
					 reverse=True)
```


----


## List Comprehensions

We do this when we want to transform a list into another list:

```python
even_numbers = [x for x in range(5) if x % 2 == 0] squares = [x * x for x in range(5)] even_squares = [x * x for x in even_numbers]
```


You can also turn a **list into a dict** this way:

```python
square_dict = { x : x * x for x in range(5) }
square_set = { x * x for x in [1, -1] }
```


If you don't need the variable from the list, use an `_`:

```python
zeroes = [0 for _ in even_numbers] # has the same length as even_numbers
```


You can **include multiple `for`s**:

```python
pairs = [(x, y)
				for x in range(10)
				for y in range(10)] # 100 pairs (0,0) (0,1) ... (9,8), (9,9)
```


In later `for`s, you can use the results of earlier ones:

```python
increasing_pairs = [(x, y)
										for x in range(10)
										for y in range(x + 1, 10)]
```


----


## Generators

A generator is something that you can iterate over (for us, usually using for) but whose values are produced only as needed (lazily).


```python
def lazy_range(n):
	"""a lazy version of range"""
	i=0
	while i < n:
		yield i
		i += 1

# to use the lazy range..
for i in lazy_range(10):
	do_something_with(i)
```

----


## Randomness


```python
import random
four_uniform_randoms = [random.random() for _ in range(4)]
#  [0.8444218515250481,    # random.random() produces numbers
#   0.7579544029403025,    # uniformly between 0 and 1
#   0.420571580830845,     # it's the random function we'll use
#   0.25891675029296335]   # most often
```

___Note:___ The random module actually produces pseudorandom (that is, deterministic) numbers based on an internal state that you can set with random.seed if you want to get reproducible results:

```python
random.seed(10)        # set the seed to 10
print random.random()  # 0.57140259469
random.seed(10)        # reset the seed to 10
print random.random()  # 0.57140259469 again
```


To __pick a number randomly from a range__:

```python
random.randrange(10) # choose randomly from range(10) = [0, 1, ..., 9]
random.randrange(3, 6) # choose randomly from range(3, 6) = [3, 4, 5]
```


To __shuffle__ a list of of items __randomly__:

```python
up_to_ten = range(10)
random.shuffle(up_to_ten)
print up_to_ten
# [2, 5, 1, 9, 7, 3, 8, 6, 4, 0] (your results will probably be different)
```


To __randomly pick__ an element from a list:

```python
my_best_friend = random.choice(["Alice", "Bob", "Charlie"]) # "Bob" for me
```


And if you need to randomly choose a sample of elements _without replacement_ (i.e., with no duplicates), you can use random.sample:

```python
lottery_numbers = range(60)
winning_numbers = random.sample(lottery_numbers, 6) # [16, 36, 10, 6, 25, 9]
```


To choose a sample of elements _with replacement_ (i.e., allowing duplicates), you can just make multiple calls to `random.choice`:

```python
four_with_replacement = [random.choice(range(10)) for _ in range(4)] # [9, 4, 4, 2]
```


----

## Regular Expressions


```python
print all([															 # all of these are true, because
	not re.match("a", "cat"),							 # * 'cat' doesn't start with 'a'
	re.search("a", "cat"),								 # * 'cat' has an 'a' in it
	not re.search("c", "dog"),						 # * 'dog' doesn't have a 'c' in it
	3 == len(re.split("[ab]", "carbs")),   # * split on a or b to ['c','r','s']
	"R-D-" == re.sub("[0-9]", "-", "R2D2") # * replace digits with dashes
]) # prints True
```


----


## Object-Oriented Programming


Imagine we didn’t have the built-in Python set. Then we might want to create our own Set class.

```python
# by convention, we give classes PascalCase names
class Set:
	# these are the member functions
	# every one takes a first parameter "self" (another convention)
	# that refers to the particular Set object being used
	def __init__(self, values=None):
		self.dict = {}    # each instance of Set has its own dict property
											# which is what we'll use to track memberships
		if values is not None:
			for value in values:
				self.add(value)

	def __repr__(self):
		"""this is the string representation of a Set object
		if you type it at the Python prompt or pass it to str()"""
		return "Set: " + str(self.dict.keys())

	# we'll represent membership by being a key in self.dict with value True
	def add(self, value):
		self.dict[value] = True

	# value is in the Set if it's a key in the dictionary				
	def contains(self, value):
		return value in self.dict

	def remove(self, value):
		del self.dict[value]
```

Which we would then use like this:

```python
s = Set([1,2,3])
s.add(4)
print s.contains(4)
s.remove(3)
print s.contains(3)
```

----


## Functional Tools


When passing functions around, sometimes we’ll want to partially apply (or curry) functions to create new functions. As a simple example, imagine that we have a function of two variables:

```python
def exp(base, power):
	return base ** power

# We can, of course, do this with def, but this can sometimes get unwieldy:

def two_to_the(power):
	return exp(2, power)
```


It's a better approach to use `functools.partial`:


```python
from functools import partial
two_to_the = partial(exp, 2) 	# is now a function of one variable
print two_to_the(3) 					# 8
```

You can also use partial to fill in later arguments if you specify their names:

```python
square_of = partial(exp, power=2)
print square_of(3)
```


We will also occasionally use `map`, `reduce`, and `filter`, which provide functional alternatives to list comprehensions:

```python
def double(x):
	return 2 * x

xs = [1,2,3,4]
twice_xs = [double(x) for x in xs] 	# [2, 4, 6, 8]
twice_xs = map(double, xs) 					# [2, 4, 6, 8]
list_doubler = partial(map, double)
twice_xs = list_doubler(xs)					# [2, 4, 6, 8]
# they all give the same result
```


You can use map with multiple-argument functions if you provide multiple lists:

```python
def multiply(x, y):
	return x * y
products = map(multiply, [1, 2], [4, 5]) # [1 * 4, 2 * 5] = [4, 10]
```


Similarly with `filter`...

```python
def is_even(x):
	"""True if x is even, False if x is odd"""
	return x % 2 == 0

x_evens = [x for x in xs if is_even(x)] # [2, 4]
x_evens = filter(is_even, xs)						# [2, 4]
list_evener = partial(filter, is_even)  # [2, 4]
x_evens = list_evener(xs)
```

----


## Enumerate


```python
# not Pythonic
for i in range(len(documents)):
	document = documents[i]
	do_something(i, document)

# also not Pythonic
i=0
for document in documents:
	do_something(i, document)
	i+=1
```


The Pythonic solution is `enumerate`, which produces tuples (index, element):


```python
for i, document in enumerate(documents):
	do_something(i, document)
```


Similarly, if we just want the indexes:

```python
for i in range(len(documents)): do_something(i)    # not Pythonic
for i, _ in enumerate(documents): do_something(i)  # Pythonic
```

----


## `zip` and Argument Unpacking


```python
list1 = ['a', 'b', 'c']
list2 = [1, 2, 3]
zip(list1, list2) # is [('a', 1), ('b', 2), ('c', 3)]
```

**To `unzip`**:

```python
pairs = [('a', 1), ('b', 2), ('c', 3)]
letters, numbers = zip(*pairs)  # returns [('a','b','c'), ('1','2','3')]
```


----


## `args` and `kwargs`

```python
def magic(*args, **kwargs): print "unnamed args:", args print "keyword args:", kwargs
    magic(1, 2, key="word", key2="word2")
    # prints
    #  unnamed args: (1, 2)
    #  keyword args: {'key2': 'word2', 'key': 'word'}
```


Doubler implemented the correct way:

```python
def doubler_correct(f):
	"""works no matter what kind of inputs f expects"""
	def g(*args, **kwargs):
  	"""whatever arguments g is supplied, pass them through to f"""
		return 2 * f(*args, **kwargs)
	return g

g = doubler_correct(f2)
	print g(1, 2) # 6
```
