# Coding good practices - 1

Maybe you just want to code. Now! Get things done. **FAST!!**  
Why would you care about how the code looks like, as long as it does what it is supposed to do?

Well, every experienced programmer, data scientist, software developer will probably tell you the same thing.  
When you program you usually have to **read** a lot of code, more than you might think. Not just write and run it.  
One reason of this is that coding often goes iteratively, which simply means that you will keep extending, fixing, improving your own code over time.
Another reason is that you will most likely also use a lot of code written by others. And, wouldn't it be nice if others could also run your code when needed?

Either way, imagine you wrote a bunch of awesome functions in Python a year ago and now you want to work on it again. The function you find is this one:
```python
def handle_x(x):
    b = 9 / 5
    c = 32
    d = 273.15
    e = x - d
    return e * b + c
```
## What does `handle_x()` do?
Not easy to spot, right? And that is still a very, very simple function.  
If you did a lot of physics you might have spotted `273.15` in the code. Mhh.... was it something with temperature?  
Still, what exactly does it do?

Maybe you can figure it out.  
Or maybe you can't.  
It actually doesn't matter, because the point is that **it will take you much longer than it should** to understand what this short, simple function is all about.
Just imagine someone gave you their "brilliant code" and it would be hundreds or thousands of lines of such functions... probably better to start re-writing the thing.


So what about the same function written like this:
```python
def kelvin_to_fahrenheit(temp_kelvin):
    fahrenheit_factor = 9 / 5
    fahrenheit_offset = 32
    absolute_zero = 273.15
    temp_celsius = temp_kelvin - absolute_zero
    return temp_celsius * fahrenheit_factor + fahrenheit_offset
 ```
 Ohhhh. That looks better. Already from the function name you will know what it does: convert a temperature in kelvin to fahrenheit!  
 Nice.  
 And if you want to understand what it really does (or how it does things), you won't have to figure out what `b, c, d, e, x` are,
 but can make much better guesses from the variable names.
 
 Ideally, you could also add a `docstring` which is a sort of build-in documentation to the function, and also give the user an idea of what kind of input is actually expected.
  This could look a bit like this:
 
 ```python
 def kelvin_to_fahrenheit(temp_kelvin: float):
    """Convert a temperature in kelvin to fahrenheit.
    """
    fahrenheit_factor = 9 / 5
    fahrenheit_offset = 32
    absolute_zero = 273.15
    temp_celsius = temp_kelvin - absolute_zero
    return temp_celsius * fahrenheit_factor + fahrenheit_offset
```
For this short and relatively easy-to-understand function, a good function name (and good variable names) might already be good enough and the docstring we show here does not add much extra information.  
But imagine much longer, more complex functions. There, a docstring could also contain detailed explanations on what to enter and how to use the function. It could even contain a code example or links to websites, references etc.
