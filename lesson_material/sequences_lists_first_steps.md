# First Steps with lists and sequences

```python
s = "Dies ist ein string"
print(s[0]) # => D

print(s[-1]) # => g

print(len(s)) # => 19
```

```python
fruit = ["apple", "orange", "banana", "pear"]
print(fruit[2])  # => banana

my_list = ["a string", 52, -0.014, "another string"]
print(my_list[0])  # => a string

my_list = [3, [7, 2], "example"]
print(my_list[1])  # => [7, 2]
```

## Tuples

```python
fruit_tuple = ("apple", "orange", "banana", "pear")
print(fruit_tuple[2])  # => "banana"
```

### What's the difference?
```python
fruit = ["apple", "orange", "banana", "pear"]
fruit[-1] = "mango"
print(fruit)  # => ['apple', 'orange', 'banana', 'mango']
```

<!--pytest-codeblocks:expect-error-->
```python
fruit_tuple[-1] = "mango"
```

[add table with operators here]

```python
my_list = [1, 4, 6, 20, 11, 99, 13, 1050]
my_str = "Ein String ist auch eine Sequenz."

4 in my_list # => True
"z" in my_str # => True

my_list[1:3] # => [4, 6]
my_str[1:3] # => in
```
