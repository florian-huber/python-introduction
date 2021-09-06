# Data types

Let's start simple, with numbers.
## Numbers

Numbers in Python mostly work as one intuitively would expect.
We often distinguish between interger (int) and float (from floating point number).
```py
>>> 5
5

>>> 5.01
5.01
```

Something about signs
```python
# + positive sign
+5  # 5

# - negative sign
-5  # -5
```

Basic math operations work as expected.
```python
# + addition
5 + 7  # 12

# - subtraction
5 - 7  # -2

# * multiplication
5 * 7  # 35

# / division
5 / 2  # 2.5
```

But careful! Divisions return floats.
This
```python
print(type(5 / 1))
```
should return:
<!--pytest-codeblocks:expected-output-->
```
<class 'float'>
```
This examples also shows how you can get the type of something in Python using `type()`.

### Combining calculations and printing

No surprise, you can of course also combine multiple such calculations.
And it is also possible to work with `print()` to output the results.
```python
print(22 * 27 * 7 / 2)
```
Should return
<!--pytest-codeblocks:expected-output-->
```
2079.0
```

It is also possible to use additional brackets for such calculations, for instance run
```python
print((22 * 27 + 12) / 2)
print(22 * 27 + 12 / 2)
```
which should give
<!--pytest-codeblocks:expected-output-->
```
303.0
600.0
```
**By the way:** It is totally fine to also use brackets to make things easier to read even though they might not be (mathematically) needed:
```python
print((22 * 27 * 7) / 2)  # that's completely fine as well!
```


Other common operations with numbers:
```python
# // integer division (ganzzahlige Division)
5 // 2  # 2 (this is NOT the same as rounding!)

# % modulo
5 % 2  # 1

# ** power (Potenz)
10 ** 2  # 100
```

## Strings

Strings are a sequence of characters and in Python are written within `'` or `"`.
```
>>> "5 + 7"
'5 + 7'
```
```
>>> "Whatever you want to type. 123*..."
'Whatever you want to type. 123*...'
```

Time for a quiz: Which of the following code samples do NOT give an error?
```
>>> “five” + “five”

>>> “five” + 5

>>> 2 * “five”

>>> “five” - “ive”
```
Nope, no solutions here... just try it out yourself.


### Handling strings (very basics)
There is a huge amount of things to know and to learn about handling strings in Python. Most of it will come later, so let's just look at some basics for now.

Strings can be added to other strings.

But additing a string to an integer or a float won't work:
<!--pytest-codeblocks:expect-error-->
```python
"100" + 5  # => TypeError
```
Makes sense.
But wait... why does Python now what is an `integer (int)`, a `float`, or a `string (str)`?

Well, that's called **duck typing** which essentially means that Python automatically assigns the type that the input resembles to.
(*"When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck."*, see [Duck test (wikipedia](https://en.wikipedia.org/wiki/Duck_test#History)

### Changing types
In some cases you might want to change the type, for instance make an `integer` from a `float`:
```python
print(5 + int(7.00001))
```
<!--pytest-codeblocks:expected-output-->
```
12
```
Only be careful that this is **NOT** the same as rounding!
```
>>> int(12.9)
12
```
(if you want to round properly run `int(round(12.9))`, but we'll cover more math functions later in the course)

Or you might want to get a number from a string:
```
>>> 5 + int("19")
24
```

Or you might want to make a string of a number, for instance to add it to another string.
```python
print("7 + 5 = " + str(7 + 5))
```
<!--pytest-codeblocks:expected-output-->
```
7 + 5 = 12
```
