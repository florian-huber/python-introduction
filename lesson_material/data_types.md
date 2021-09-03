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
```console
type(5 / 1)
```
should return:
<!--pytest-codeblocks:expected-output-->
```
float
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
