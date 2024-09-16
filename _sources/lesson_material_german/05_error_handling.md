# Error handling
Bestimmt haben alle inzwischen schon einige Python Errors gesehen...

z.B. 
<!-- pytest-codeblocks:expect-error -->
```python
print(dies)  # => NameError: name 'dies' is not defined
```

oder
<!-- pytest-codeblocks:expect-error -->

```python
else = 5  # => SyntaxError: invalid syntax
```

oder
<!-- pytest-codeblocks:expect-error -->
```python
open("no_file")  # => FileNotFoundError: [Errno 2] No such file or directory: 'no_file'
```

oder
<!-- pytest-codeblocks:expect-error -->
```python
5 / 0  # => ZeroDivisionError: division by zero
```

Einige davon sind Fehlermeldungen die eigentlich nie vorkommen sollten
und wirklich auf Fehler im geschriebenen Code hinweisen, z.B.
`else = 5` was in Python (zum Glück) nicht erlaubt ist!
Fehler können allerdings auch als Teil laufender Programme auftreten 
und nicht immer wollen wir das dadurch die Programmausführung stoppt.

```python
def divider(zahl, geteilt_durch):
    """Teilt zahl durch geteilt_durch.
    
    Bei Division durch 0 wird None ausgegeben.
    """
    if geteilt_durch == 0:
        return None
    return zahl / geteilt_durch

print(divider(5, 0))
```

Das Problem dabei: Nicht immer sind Fehler so einfach vorhersehbar.
Besser ist darum den Fehler "abzufangen". Das geht in Python mit `try: except`

```python
try:
    print(dies)
except:
    print("Programm lief durch bis hier------")
```
oder so:
```python
try:
    print(dies)
except Exception as error:
    print(error)
print("Programm lief durch bis hier------")
```

Zurück zum divider Programm:

```python
def divider(zahl, geteilt_durch):
    """Teilt zahl durch geteilt_durch.
    
    Bei Division durch 0 wird None ausgegeben.
    """
    try:
        return zahl / geteilt_durch
    except ZeroDivisionError:
        return None

print(divider(5, 0))
```

> ### Mini Quiz:
> Was macht diese funktion wenn sie aufgerufen wird über
> `print(divider("text", 2))`
>  
> a) None  
> b) te  
> c) TypeError


### Häufige Fehlertypen

+ IOError – wenn z.B. eine Datei nicht geöffnet werden kann.
+ ImportError – wenn ein Python-Modul nicht gefunden oder geladen werden kann.
+ ValueError – wenn eine Funktion ein Argument mit richtigem Typen aber falschem Inhalt erhält.
+ TypeError - für Argumente mit nicht definiertem/zugelassenen Typen.
+ KeyboardInterrupt – wenn der Nutzer übers Keyboard abbricht (Control-C)


Manchmal wollen wir auch bewusst Fehlermeldungen produzieren
Das geht zum einen mit `raise`

<!-- pytest-codeblocks:expect-error -->

```python
raise Exception("Something went wrong")
```

<!-- pytest-codeblocks:expect-error -->

```python
x = "hello"

if not type(x) is int:
    raise TypeError("Only integers are allowed") 
```


## Assert
Oft ist es in solchen Fällen einfacher mit einer anderen Python Funktion zu arbeiten: ´assert´

<!-- pytest-codeblocks:expect-error -->

```python
selected_number = "hello"
assert isinstance(selected_number, int), "Excepted integer"
```

Beispiel:

```python
def hour_to_am_pm(hour):
    """Convert 24-hours to 12-hour clock.
    """
    assert 0 <= hour <= 24, "hour must be between 0 and 24"
    
    if 1 <= hour <= 12:
        print(f"{hour} a.m.")
    elif hour == 0:
        print("12 a.m.")
    else:
        print(f"{hour - 12} p.m.")


hour_to_am_pm(13)  # => 1 p.m.
```

<!-- pytest-codeblocks:expect-error -->

```python
hour_to_am_pm(25)  # => AssertionError: hour must be between 0 and 24
```

