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
