# Useful Functions

It's about time that we wrote some Python! Everything from here on out is about
tricks in Python that will help you in 6.101.

## String Manipulation

Strings in Python have a super useful library of methods that you can call on
them, and this section will explain some of them. For the full list, see
[this link](https://docs.python.org/3/library/stdtypes.html#string-methods)

### [split], [join], [replace]

[split], [join], and [replace] are three string functions you ought to know.

**[split]** is simple. It splits a string into a list of strings by a certain
character. By default, it will just split on spaces, though you can specify a
character to split on.

```pycon
>>> "1 2 3".split()
['1', '2', '3']

>>> "1,2,3".split(",")
['1', '2', '3']
```

**[join]** does the exact opposite. It inserts a string in between strings in an
iterable[^2]. This is helpful when you have a list of strings (or list
comprehension) that you want to merge together into a string. You can use the
empty string (`""`), if you want to join the elements together without anything
in between.

```pycon
>>> ",".join(["a", "b", "c"])
'a,b,c'

>>> "".join(['x', 'y', 'z'])
'xyz'

>>> " -> ".join(str(i) for i in range(10))
'0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9'
```

**[replace]** is also super simple. If it finds an occurrence of a string, it
replaces it with the new value and returns the modified string. If it doesn't
find it, it returns the original string.

```pycon
>>> "This guide is for 6.100 students.".replace("100", "101")
'This guide is for 6.101 students.'

>>> "Hello!".replace("Hi", "Howdy")
'Hello!'
```

[^2]:
    An iterable is just anything that can be iterated on. It includes lists,
    tuples, generators, or classes that implement the `__iter__` method.

[split]: https://docs.python.org/3/library/stdtypes.html#str.split
[join]: https://docs.python.org/3/library/stdtypes.html#str.join
[replace]: https://docs.python.org/3/library/stdtypes.html#str.replace

### [f-strings]

[F-strings], or Formatted String Literals, in Python let you embed the value of
variables in strings in your program. This concept is called [string
interpolation].

The most basic use of [f-strings] requires putting an `f` before the start of
your Python string, and then, you can embed variables in your string using curly
braces (`{}`).

```pycon
>>> name = "Cameron"

>>> f"Hello, {name}!"
'Hello, Cameron!'
```

Putting an equals sign after the variable name prints out the name of the
variable as well, and this can be extremely useful when you're debugging.

```pycon
>>> f"Variables: {name=}"
"Variables: name='Cameron'"
```

[f-strings]:
  https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals
[string interpolation]: https://en.wikipedia.org/wiki/String_interpolation

## List Utilities

Python has a ton of super useful built-in functions that you should know about.
A comprehensive list can be found
[here](https://docs.python.org/3/library/functions.html), but we'll be going
into detail about some of them below.

Many of these utilities reference Python **iterables**. An iterable just means
anything you can iterate over, and this includes lists, sets, tuples, dicts,
generators, and any class that implements the `__iter__` method. These methods
should help save you from writing tons of for loops.

### [min], [max]

**[min]** finds the minimum of multiple elements, and **[max]** finds the
maximum. They take anything that can be compared, like `int`s and `float`s, or
you can pass them strings to use alphabetical order. They can be used in two
different ways. You can pass in multiple arguments, like so:

```pycon
>>> max(7, 5, 8, 5.5)
8

>>> min("banana", "cat", "apple")
'apple'
```

You can also give an iterable of items, like a list, set, tuple, or
comprehension, to find the minimum or maximum of as a single argument.

```pycon
>>> max([7, 5, 8, 5,5])
8

>>> min(("banana", "cat", "apple"))
'apple'
```

[min]: https://docs.python.org/3/library/functions.html#min
[max]: https://docs.python.org/3/library/functions.html#max

### sorted, reversed

[sorted] returns a copy of an iterable (like a list or tuple) as a sorted list,
while [list.sort] and list.reverse work in-place.

```pycon
>>> sorted(["python", "typescript", "html"])
['html', 'python', 'typescript']
```

[reversed] returns an iterable, so you need to call [list] or [tuple] on it in
order to convert it to something you can use.

```pycon
>>> reversed(("first", "second", "third"))
<reversed object at 0x10099b490>
>>> list(reversed(("first", "second", "third")))
['third', 'second', 'first']
```

[list]: https://docs.python.org/3/library/stdtypes.html#list
[tuple]: https://docs.python.org/3/library/stdtypes.html#tuple
[sorted]: https://docs.python.org/3/library/functions.html#sorted
[reversed]: https://docs.python.org/3/library/functions.html#reversed
[list.sort]: https://docs.python.org/3/library/stdtypes.html#list.sort

### The `key` argument

If you have some data that can't be compared directly, [min] and [max] (as well
as [sorted] and [reversed] below) both take a `key=` argument, which allows you
to specify a lambda function that returns a value that will be compared instead.
This works especially well for tuples of data.

```pycon
>>> max(("foo", 4), ("bar", 7), ("baz", 2), key=lambda v: v[1])
('bar', 7)

>>> min("longer string", "long string", "string", "str", key=len)
'str'
```

We can also call [sorted] with the `key` argument.

```pycon
>>> sorted(["longer string", "long string", "string", "str"], key=len)
['str', 'string', 'long string', 'longer string']
```

If you want to sort in place, [list.sort] also accepts the same `key` argument.

```pycon
>>> data = [("foo", 4), ("bar", 7), ("baz", 2)]
>>> data.sort(key=lambda v: v[1])
>>> data
[('baz', 2), ('foo', 4), ('bar', 7)]
```

Sorting (or using min or max) tuples _without_ a `key` parameter sorts based on
the first element in the tuples.

```pycon
>>> data.sort()
>>> data
[('bar', 7), ('baz', 2), ('foo', 4)]
```

### [any], [all]

**[any]** and **[all]** are functions that check if _any_ or _all_ values in an
iterable are truthy.[^3] Here's some examples:

```pycon
>>> any([True, False, False])
True
>>> True or False or False  # The same as combining the elements with or
True
```

```pycon
>>> all([True, False, False])
False
>>> True and False and False  # The same as combining the elements with and
False
```

These can be especially helpful with comprehensions, or types that aren't
booleans.

```pycon
>>> all(len(s) > 3 for s in ["apple", "banana", "kiwi"])
True
>>> any([0, 2, 0])
True
```

[all]: https://docs.python.org/3/library/functions.html#all
[any]: https://docs.python.org/3/library/functions.html#any

[^3]:
    If you need a refresher on what a truthy value in Python means, check out
    [this tutorial](https://www.freecodecamp.org/news/truthy-and-falsy-values-in-python/).

### [sum]

**[sum]** takes in an iterable and sums up its components. It takes
comprehensions as well, so it comes in handy often. Try to use it instead of a
for loop when you can.

```pycon
>>> sum([1,2,3])
6
>>> sum(i for i in range(10))  # Sum of 1 to 10
45
```

[sum]: https://docs.python.org/3/library/functions.html#sum

### [zip], [enumerate]

**[zip]** takes in any number of iterables and combines them together into a
single iterable of tuples. You need to convert it to a [list] or a [tuple] if
you're not going to iterate over it. This is helpful if you have two or more
lists that correspond to each other and want to iterate over them at the same
time.

```pycon
>>> labels = ['apples', 'bananas', 'kiwis']
>>> inventory = [12, 30, 5]
>>> zip(labels, inventory)
<zip object at 0x100c94f80>
>>> list(zip(labels, inventory))
[('apples', 12), ('bananas', 30), ('kiwis', 5)]
```

What's cool is that this is the same format as [dict.items], so the [dict]
constructor takes in key-value pairs in this format. So, it's super easy to make
a dictionary from our zip output.

```pycon
>>> dict(zip(labels, inventory))
{'apples': 12, 'bananas': 30, 'kiwis': 5}
```

Have you ever been iterating over a list and wanted its indices as well as its
values? **[enumerate]** is just the function you need. It takes in an iterable
and returns an iterable of tuple pairs `(index, value)`.

```pycon
>>> list(enumerate(labels))
[(0, 'apples'), (1, 'bananas'), (2, 'kiwis')]
>>> for index, label in enumerate(labels):
...     print(f"{index}: {label}")
0: apples
1: bananas
2: kiwis
```

[zip]: https://docs.python.org/3/library/functions.html#zip
[enumerate]: https://docs.python.org/3/library/functions.html#enumerate
[dict]: https://docs.python.org/3/library/stdtypes.html#dict
[dict.items]: https://docs.python.org/3/library/stdtypes.html#dict.items

### Tuple unpacking

> **PLEASE USE THIS, THANKS.**
>
> ❤️ LOVE, YOUR 6.101 LAs

Tuple unpacking is an incredibly useful feature that lets you break apart tuples
into their elements and assign each element in a tuple to a separate variable.
If you have a tuple, you can separate variables by commas to assign each
variable to that element in a tuple, like so. This behavior is referred to as
"unpacking" the tuple.

```pycon
>>> number, letter = (1, "A")
>>> number
1
>>> letter
'A'
```

This feature gets useful once we use it to work with data that's structured in
tuples. Without tuple unpacking, you might be inclined to access the flight
information in this example by indexing into the tuple, like this:

```pycon
>>> flights = [(1234, "BOS", "LAX"), (5678, "JFK", "ATL")]
>>> for flight in flights:
...     print(f"Flight {flight[0]} departs {flight[1]} for {flight[2]}")
Flight 1234 departs BOS for LAX
Flight 5678 departs JFK for ATL
```

However, a much more readable option would be to break apart, or "unpack", the
flight tuple in the for loop, like this:

```pycon
>>> for flight in flights:
...     num, origin, dest = flight
...     print(f"Flight {num} departs {origin} for {dest}")
Flight 1234 departs BOS for LAX
Flight 5678 departs JFK for ATL
```

Now, `num` is always assigned to the flight number, `origin` is always assigned
to the flight's origin, and `dest` is always assigned to the flight destination.
This makes the code much more readable for when anyone is debugging your code.

Python also has a helpful shorthand for this, which moves the unpacking into the
first line of the for loop, like this:

```pycon
>>> for num, origin, dest in flights:
...     print(f"Flight {num} departs {origin} for {dest}")
Flight 1234 departs BOS for LAX
Flight 5678 departs JFK for ATL
```

## [dict] and [set] Utilities

As you'll learn in 6.101, **[dict]** and **[set]** in Python have special
properties that make them useful. For a dictionary, we can map arbitrary keys to
arbitrary values, so that it's extremely fast to look up and retrieve a value.
In sets, we can check inclusion extremely quickly to see if a given value is in
a set.

In this section, you'll learn some handy tricks for working with [dict] and
[set].

[dict]: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
[set]: https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset

### frozenset

Unfortunately, we can only put [immutable] objects inside sets. Because sets are
mutable, we can't put sets inside sets, which is something you might want to do
in a 6.101 lab or two. [frozenset] fixes that problem for us because frozensets
are immutable, so we can put them inside sets.

You can create a [frozenset] with its constructor from any iterable, like
another set, list, generator, or even a string.

```pycon
>>> alphabet = frozenset("abcdefghijklmnopqrstuvwxyz")
>>> alphabet
frozenset({'q', 'm', 'w', 'x', 'h', 's', 't', 'a', 'n', 'j', 'c', 'f', 'g', 'u', 'd', 'l', 'k', 'p', 'r', 'o', 'e', 'b', 'z', 'v', 'i', 'y'})
```

Say I have a class of students that I arrange into groups of two. The specifics
of how I make the groups don't matter all that much, just that we have a list of
lists at this point.

```pycon
>>> students = ["John", "Jack", "Sally", "Susan", "Alice", "Bob"]
>>> random.shuffle(students)
>>> groups = [students[i:i+2] for i in range(0, len(students), 2)]
>>> groups
[['John', 'Susan'], ['Bob', 'Sally'], ['Alice', 'Jack']]
```

If I want to find out whether John and Susan are paired up together, I need to
do a couple things:

1. First, I have to loop over all the groups in the list, which could be very
   slow if there are a lot of groups.
2. Next, for each group, I need to check if both John and Susan are in it,
   considering that they could be in any order.

A better approach would be to convert all the inner lists to sets, like this:

```pycon
>>> groups = [set(g) for g in groups]
>>> groups
[{'John', 'Susan'}, {'Bob', 'Sally'}, {'Alice', 'Jack'}]
>>> {'Susan', 'John'} in groups
True
>>> {'John', 'Susan'} in groups
True
```

You'll notice that we can use this more elegant syntax with the **[in]**
operator, and it doesn't matter what order "John" and "Susan" are when we check.
This is more convenient, but the in operator on lists loops over the list to
check for inclusion, which could be slow if the list is very long.

**The best approach is to use frozensets inside sets.** Fortunately, this is
super easy to adopt.

```pycon
>>> groups = {frozenset(g) for g in groups}
>>> groups
{frozenset({'Alice', 'Jack'}), frozenset({'John', 'Susan'}), frozenset({'Bob', 'Sally'})}
>>> {'John', 'Susan'} in groups
True
>>> {'Susan', 'John'} in groups
True
```

This is way faster because each `in` check takes a constant amount of time,
regardless of how full the set is. Notice also that we can still use regular
sets to check inside our list of frozensets. This is because Python treats
frozensets and sets the same, and you can even check if their contents are
equal:

```pycon
>>> {'a'} == frozenset({'a'})
True
```

[immutable]: https://en.wikipedia.org/wiki/Immutable_object
[frozenset]: https://docs.python.org/3/library/stdtypes.html#frozenset
[in]: https://docs.python.org/3/library/stdtypes.html#common-sequence-operations

### [dict.get]

**[get][dict.get]** is a [dict] method we can use to retrieve a value from a
dictionary. But you might be wondering, can't we already do that with `d[key]`?
The difference is that [dict.get] doesn't raise a [KeyError] when `key` isn't in
the dictionary (it returns `None` instead), and we can even specify a default
value to get returned.

```pycon
>>> inventory = {'carrots': 3, 'cucumbers': 7}
>>> inventory['peppers']  # booo, KeyError :(
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'peppers'
>>> inventory.get('peppers')  # No KeyError, just None!
>>> inventory.get('peppers', 0)  # No KeyError, now we get 0!
0
```

[dict.get]: https://docs.python.org/3/library/stdtypes.html#dict.get
[keyerror]: https://docs.python.org/3/library/exceptions.html#KeyError

### [dict.setdefault]

**[setdefault][dict.setdefault]** comes in handy when we have dictionaries with
mutable values that we want to either initialize or change. Say I have a list of
flights, and I'm trying to make a dictionary mapping origins to the
destinations:

```pycon
>>> flights = [('BOS', 'JFK'), ('BOS', 'ATL'), ('JFK', 'LAX'), ('LAX', 'SFO'), ('BOS', 'LAX'), ('JFK', 'DFW')]
>>> destinations = {}
>>> for origin, dest in flights:
...     if origin in destinations:
...             destinations[origin].add(dest)
...     else:
...             destinations[origin] = {dest}
>>> destinations
{'BOS': {'ATL', 'JFK', 'LAX'}, 'JFK': {'DFW', 'LAX'}, 'LAX': {'SFO'}}
```

This loop is made way more complicated by having to check whether we need to
create a new set in the dictionary for a given key. [dict.setdefault] lets us
combine these two steps into one by creating a new set for us automatically if
one doesn't exist already.

```pycon
>>> for origin, dest in flights:
...     destinations.setdefault(origin, set()).add(dest)
>>> destinations
{'BOS': {'ATL', 'JFK', 'LAX'}, 'JFK': {'DFW', 'LAX'}, 'LAX': {'SFO'}}
```

[dict.setdefault]:
  https://docs.python.org/3/library/stdtypes.html#dict.setdefault

### The `|` operator, [dict.update], and [set.update]

The `|` operator is the "intersection" operator for sets and dictionaries in
Python. In short, it lets you merge together two sets, or two dictionaries. In
the case of a [dict], the values in the right dict take precedence.

```pycon
>>> {'a'} | {'b', 'c'}
{'b', 'a', 'c'}
>>> {'a': 1} | {'b': 2, 'a': 3}
{'a': 3, 'b': 2}
```

Like with other operators in Python, we can combine the intersection operator
with the `=` assignment operator, using the `|=` operator to "update" a
dictionary or set with new values.

```pycon
>>> s = {1,2,3}
>>> s |= {4,5,6}
>>> s
{1, 2, 3, 4, 5, 6}
>>> d = {'a': 1}
>>> d |= {'b': 2}
>>> d
{'a': 1, 'b': 2}
```

The **[dict.update]** and **[set.update]** methods are identical to using the
`|=` operator.

```pycon
>>> s.update({7,8,9})
>>> s
{1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> d.update({'c':3})
>>> d
{'a': 1, 'b': 2, 'c': 3}
```

[dict.update]: https://docs.python.org/3/library/stdtypes.html#dict.update
[set.update]: https://docs.python.org/3/library/stdtypes.html#frozenset.update

### [dict.items]

**[dict.items]** is a method that yields elements in a dictionary as key-value
pairs, and you should use it if you need to access both the keys and values of a
dictionary in a for loop. Here's how it works:

```pycon
>>> inventory = {'carrots': 3, 'cucumbers': 7, 'broccoli': 10}
>>> for key, value in inventory.items():
...     print(f"There are {value} pieces of {key} in the store.")
There are 3 pieces of carrots in the store.
There are 7 pieces of cucumbers in the store.
There are 10 pieces of broccoli in the store.
```

[dict.items]: https://docs.python.org/3/library/stdtypes.html#dict.items