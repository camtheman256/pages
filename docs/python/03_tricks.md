# Trick Sheet

This page contains concise ways to manipulate data in Python that can save you
time and code.

## Clamp Function

A **clamp function** (`clamp(x, minimum, maximum)`) lets you constrain the value
of an input to a range. If I want to clamp a value _x_ between 1 and 5, the
outputs of clamp(_x_, 1, 5) would look like:

|   x | clamp(x, 1, 5) |
| --: | :------------- |
|   0 | 1              |
|   1 | 1              |
|   4 | 4              |
|   8 | 5              |

Writing this kind of function will be helpful in all kinds of places in 6.101. A
straightforward way we could write this is by checking whether the value is
below the min or above the max and returning the appropriate boundary.

```py
def clamp(x, minimum, maximum):
    """
    Returns x if minimum < x < maximum. Otherwise, returns the appropriate
    boundary.
    """
    if x < minimum:
        return minimum
    if x > maximum:
        return maximum
    return x
```

However, notice that the built-in
[min and max functions](./02_functions.md#min-max) achieve a one-sided clamp,
half the work of our two-sided clamp above.

```py
def clamp_left(x, minimum):
    """
    Returns x if x > minimum. Otherwise, returns minimum.
    """
    return max(x, minimum)

def clamp_right(x, maximum):
    """
    Returns x if x < maximum. Otherwise, returns maximum.
    """
    return min(x, maximum)
```

It turns out we can combine `min` and `max` together to make an elegant clamp
function by just nesting one inside the other! I like to structure the clamp
with the `max` function on the outside and the minimum value on the left, so
that the clamp looks like the statement `minimum < x < maximum`.

```py
def clamp(x, minimum, maximum):
    """
    Returns x if minimum < x < maximum. Otherwise, returns the appropriate
    boundary.
    """
    return max(minimum, min(x, maximum))
```

You don't need to write this as a function if you don't want to! Say we have a
list `data` of variable length. Now, we have a nice way to make sure whatever
index we provide can access a valid data point.

```py
def access_data(index):
    """Returns the nearest data point to index."""
    return data[max(0, min(index, len(data)))]
```

### Range Check

Sometimes we just want to check if a value is in a range instead of clamping it.
A simple way to do this in Python is with a comparison. Operators like `<` and
`==` wil let you string together three values in a single comparison.

If we want to check whether x is within 1 and 5, we can use `#!py 1 <= x <= 5`,
like so:

```pycon
>>> x = 10
>>> 1 <= x <= 5
False
>>> x = 3
>>> 1 <= x <= 5
True
```
