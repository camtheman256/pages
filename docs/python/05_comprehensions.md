# Comprehension

Comprehension in Python is a powerful shorthand tool, and it's worth taking a
minute to learn how to wield it.

## Comprehension Structure

Comprehensions in Python have three main components:

1. `expression` - an expression that evaluates to whatever gets put in the
   iterable
2. `for iterator in iterable` - a for loop that controls the iteration
3. _(optionally)_ `if condition` - an if statement that controls whether that
   value should get included in the iterable

Putting that together, comprehensions look like:

```python
[expression for iterator in iterable if condition]
```

Comprehensions are made to take code that looks like this:

```python linenums="1"
# Creates a list of the squares of even numbers from 0 to 99
nums = []

for i in range(100):
    if i % 2 == 0:
        nums.append(i**2)
```

and turn it into an elegant one line list comprehension:

```python
nums = [i**2 for i in range(100) if i % 2 == 0]
```

If we wanted all squares from 0 to 99, we could just drop the if condition, like
this:

```python
all_nums = [i**2 for i in range(100)]
```

So, if you ever see code that just appends to a list, with or without an `if`
statement, try refactoring it into a list comprehension. You can sometimes make
your code significantly shorter and more readable by refactoring simple steps
into comprehensions.

## Types of Comprehensions

Comprehensions are such a powerful feature that Python has implemented them in
many different forms.

### List Comprehensions

The **list comprehension** is the most basic and shown in the examples above. A
list comprehension returns a new list with the results of the comprehension and
is surrounded by square brackets (`[]`).

```pycon
>>> flights = [
...     {'origin': 'BOS', 'destination': 'JFK', 'seats': 150},
...     {'origin': 'BOS', 'destination': 'ATL', 'seats': 200},
...     {'origin': 'JFK', 'destination': 'LAX', 'seats': 250},
...     {'origin': 'LAX', 'destination': 'SFO', 'seats': 100},
...     {'origin': 'BOS', 'destination': 'LAX', 'seats': 250},
...     {'origin': 'JFK', 'destination': 'DFW', 'seats': 150}
... ]
>>> routes = [(f['origin'], f['destination']) for f in flights]
>>> routes
[('BOS', 'JFK'), ('BOS', 'ATL'), ('JFK', 'LAX'), ('LAX', 'SFO'), ('BOS', 'LAX'), ('JFK', 'DFW')]
```

### Set Comprehensions

We can also make **set comprehensions** easily by surrounding the comprehension
in curly braces (`{}`).

```pycon
>>> origins = {f['origin'] for f in flights}
>>> origins
{'LAX', 'JFK', 'BOS'}
```

### Dict Comprehensions

We can also make dictionaries easily with **dict comprehensions**. These work
very similarly to set comprehensions, except we specify both a key and a value
in the _expression_ portion of the comprehension. Because dict comprehensions
are a bit complicated, here's a simpler example that uses them to map letters in
the alphabet to their index.

```pycon
>>> alphabet = "abcdefghijklmnopqrstuvwxyz"
>>> {letter: index+1 for index, letter in enumerate(alphabet)}
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6, 'g': 7, 'h': 8, 'i': 9, 'j': 10,
 'k': 11, 'l': 12, 'm': 13, 'n': 14, 'o': 15, 'p': 16, 'q': 17, 'r': 18, 's': 19,
 't': 20, 'u': 21, 'v': 22, 'w': 23, 'x': 24, 'y': 25, 'z': 26}
```

Going back to the flights data, we can use the route tuples
`(origin, destination)` as keys of a dictionary, where the values are the number
of seats on that flight.

```pycon
>>> seats_by_route = {(f['origin'], f['destination']): f['seats'] for f in flights}
>>> seats_by_route
{('BOS', 'JFK'): 150, ('BOS', 'ATL'): 200, ('JFK', 'LAX'): 250, ('LAX', 'SFO'): 100,
 ('BOS', 'LAX'): 250, ('JFK', 'DFW'): 150}
```

### Generator Comprehensions

The most general form of comprehensions is a **generator comprehension**.
Generator comprehensions can easily be converted to other types with their
constructors, such as tuple or frozenset. They can be created by surrounding a
comprehension with parentheses (`()`), but more often, you'll just want to pass
them directly to whatever data type you need them to be.

```pycon
>>> (i for i in range(10))
<generator object <genexpr> at 0x100cfb510>
>>> frozenset(l.upper() for l in alphabet)
frozenset({'N', 'C', 'Q', 'R', 'O', 'V', 'P', 'G', 'T', 'B', 'S', 'Y', 'A', 'F',
           'J', 'X', 'U', 'K', 'D', 'I', 'L', 'Z', 'W', 'E', 'H', 'M'})
>>> tuple(i**3 for i in range(10))
(0, 1, 8, 27, 64, 125, 216, 343, 512, 729)
```

**Be careful with how you use generator comprehensions!** Simply surrounding a
comprehension in parentheses (like `(i for i in range(10))`) will give you a
generator, _not a tuple!_ If you want a tuple, pass your generator comprehension
to the tuple constructor, like so:

```pycon
>>> tuple(i for i in range(10))
(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```