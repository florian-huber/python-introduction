# Variables

Variables are key to programming. Think of them as boxes for storing data values.

In Python a variable is created by assigning a value to it:
```idle
>>> a = 5
>>> a / 6
0.8333333333333334
```

As long as you have your Python session running and do anything else with `a` it will keep the value you first assigned to it (5).  
For instance if you later run this:
```idle
>>> b = 8
>>> a + b
13
```

But such variables are not fixed. You can simply go ahead and change the assigned value (can also be a different data type!):
```idle
>>> a = 100.5
>>> a + b
108.5
```

Most of the time, you should try to name your variables in a way that gives a hint on what they stand for.  
This will make code like the following much easier to understand!
```idle
>>> training_seconds = 18900
>>> sec_per_hour = 60 * 60
>>> training_hours = training_seconds / sec_per_hour
>>> print(training_hours)
5.25
```

Python is pretty flexible about the handling of variables. 
Usually that's good, but occationally it also requires that you do some effort to avoid weird things from happening ("undesired behavior").
Imagine you had a variable that was called 'print':
```idle
>>> print = "my text"
```
Python won't give you any errors when you do. But **not getting an error doesn't mean it's a good idea...**  
What do you think will now happen if you try to run code like `print("Hello, still there?")` ? 

Ok, I guess that's clear, right? Don't use variable names of common functions, or functions that you intend to use.

One thing that Python even forbids right away is calling your variable after one of the [**reserverd keywords**](https://www.w3schools.com/python/python_ref_keywords.asp).
<!--pytest-codeblocks:expect-error-->
```python
>>> or = "outer region"
SyntaxError: invalid syntax
```

## Mini quiz:

Which of the following are OK to use variable names?
```
>>> print = “my text”
>>> else = “my text”
>>> sehr_wichtig = "my text"
>>> übermäßig_wichtig = "my text"
>>> ImportantText = "my text"
```




