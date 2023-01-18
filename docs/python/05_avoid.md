# Avoid This

This section includes some pitfalls to avoid when you're writing Python that
I've seen crop up in student code in 6.101.

## Don't assign the output of functions that don't return anything

Pay attention to what functions in Python return! [list.append], [set.remove],
[set.add], and [list.sort] are some common functions that you might use in your
programs, and **they all return `None`!** Instead, they modify the underlying
object in-place. Make sure to **never** assign the output of any of these
function calls because they don't return any value.

This means this example below won't return anything and causes an error instead.

```python linenums="1"
# FIXME: DOES NOT WORK!! unsorted.sort() returns None, so this code will fail!
def sort_tuple(unsorted):
    """Takes in a tuple and returns a sorted version."""
    list_to_sort = list(unsorted)
    return tuple(list_to_sort.sort())
```

If we tried this example in our REPL, we would see this error, due to
`#!py list_to_sort.sort()` returning `None`.

```pycon
>>> unsorted = (7, 5, 3, 9, 12)
>>> sort_tuple(unsorted)
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
  File "<stdin>", line 5, in sort_tuple
TypeError: 'NoneType' object is not iterable
```

However, we can reference the original variable after calling [list.sort] on it,
and now the function will work properly.[^sort]

```python linenums="1"
def sort_tuple(unsorted):
    """Takes in a tuple and returns a sorted version."""
    list_to_sort = list(unsorted)
    list_to_sort.sort()
    return tuple(list_to_sort)
```

```pycon
>>> sort_tuple(unsorted)
(3, 5, 7, 9, 12)
```

[^sort]:
    The best way to sort tuples would be to use the [sorted] function and
    convert to a tuple, like `#!py tuple(sorted(unsorted))`, but for this
    example, we convert to a list so we can use [list.sort].

[list.append]:
  https://docs.python.org/3/library/stdtypes.html#mutable-sequence-types
[list.sort]: https://docs.python.org/3/library/stdtypes.html#list.sort
[set.add]: https://docs.python.org/3/library/stdtypes.html#frozenset.add
[set.remove]: https://docs.python.org/3/library/stdtypes.html#frozenset.remove
[sorted]: https://docs.python.org/3/library/functions.html#sorted

## Boolean Laundering / Unnecessary nesting

You might find places in your code where code can be simplified or condensed.
One way we can often reduce code is by relying on truthiness of data types. Say
we want to check whether a list `data` has any values in it to run a function.
We could do:

```python
if len(data) > 0:
    print("list has values!")
```

However, a more elegant way to do this would be to rely on the fact that lists
with elements are truthy, like so:

```python
if data:
    print("list has values!")
```

This also works for dictionaries and sets.

Another thing to look for when condensing code is **boolean laundering**, where
we manually check the value of a condition and then return `True` or `False`
based on it. See this example:

```python
def func():
    if condition:
        return True
    else:
        return False
```

Notice that if `condition` is `True`, we return `True`, and if it's `False`, we
return `False`. These 4 lines can be condensed into 1 if we just do:

```python
def func():
    return condition
```

Another thing we can check for is **unnecessary nesting** of if statements. If
we want to run a function based off of multiple conditions, you might have
several if statements nested like this:

```python
if condition1:
    if condition2:
        if condition3:
            do_thing()
```

Notice that `do_thing()` will only run if `condition1` and `condition2` and
`condition3` are all true. We can combine these with the `and` operator, like
so.[^sce]

```python
if condition1 and condition2 and condition3:
  do_thing()
```

[^sce]:
    Because of Python's short circuit evaluation of `and`, `condition2` will not
    be evaluated if `condition1` is False, so these two code samples work
    _exactly_ the same!

## When to use `i` as an iterator

In many programming languages, it's convention to use `i` as an iterator, which
is why you'll see `#!python for i in range(n)` as the first line of many for
loops in Python. However, you should only use `i` when you're iterating over a
range of integers, to match convention with other languages. If you're iterating
over other data structures, try to pick a variable name that captures what that
element represents, like in the following examples:

```python
alphabet = "abcdefghijklmnopqrstuvwxyz"
for letter in alphabet:
    pass

scores = [12, 57, 35, 17]
for i, score in enumerate(scores):
    print(f"Player {i} has {score} points.")

people = ["Cameron", "Jeff", "Sarah"]
for person in people:
    pass
```

## Return inside a loop

Python uses the indentation of your code to organize it into blocks. [This
link][indentation] has a diagram of what that looks like. This means that your
code will behave differently based on its indentation level, so you should pay
attention to how indented lines of code are.

Say we create a function to increment a counter, and we want to return the last
value.

```python linenums="1"
def count(n):
    counter = 0
    while counter < n:
        counter += 1
        return counter  # Wrong indentation level!! Returns immediately.
```

Because of how it's indented, the return statement on line 5 is _inside_ the
while loop. This means that the function will return on the first iteration of
the while loop, preventing the loop from continuing. Without changing any code,
we can change the indentation level of the last line to make it return after the
loop is finished.

```python linenums="1"
def count(n):
    counter = 0
    while counter < n:
        counter += 1
    return counter  # Correct indentation, returns at the end of the loop.
```

Be careful whenever you put `return` statements around loops, as returning a
function inside of a loop will stop its execution and might not be what you
intend to do.

[indentation]: https://www.geeksforgeeks.org/indentation-in-python/

## Making Unnecessary Copies

Functions that update an object in-place will generally be much faster another
function that returns a copy of that data structure. As an example, consider the
following code that adds all numbers under a million to a set `b`.

```python linenums="1" hl_lines="3"
b = set()
for i in range(1_000_000):
    b = b | {i,}  # SUPER SLOW!
```

Line 3 will create a new set (containing `b | {i,}`) on _every single iteration_
and reassign the `b` variable to that new set. Creating all those new sets takes
a long time to finish, and in reality, we just want to add some new values to
the `b` set.

Fortunately, we have lots of options for how to fix this line of code. An easy
one would be to replace line 3 with our handy `|=` operator from the
[Useful Functions section](02_functions.md#the-operator-dictupdate-and-setupdate).

```python
b |= {i,}
```

If you remember, this operator is identical to [set.update], so we could write
line 3 this way as well:

```python
b.update({i,})
```

Finally, we could also just use the [set.add] function you're familiar with to
add a single element to our set.

```python
b.add(i)
```

[set.update]: https://docs.python.org/3/library/stdtypes.html#frozenset.update
[set.add]: https://docs.python.org/3/library/stdtypes.html#frozenset.add
