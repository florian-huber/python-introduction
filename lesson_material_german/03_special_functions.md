# Special functions
Wir haben uns schon Funktionen in Python angeschaut. Zur Erinnerung:  
Funktionen werden mit `def` definiert. Funktionen müssen aufgerufen werden um wirklich vom Interpreter ausgeführt zu werden. Es können theoretisch beliebig viele Parameter (oder Argumente) an eine Funktion übergeben werden. Über `return` gibt die Funktion Variablen zurück und ended. Falls kein explizietes `return` statement angegeben wird, gibt eine Funktion `None` zurück.  

Im Folgenden gehen wir kurz auf einige spezielle Funktionen-Typen ein.

## Rekursive Funktionen (*recursive functions*)
Rekursive Funktionen sind Funktionen die sich selbst aufrufen.
Ein typisches Beispiel dafür ist die Berechnung von Fakultäten (engl. *factorial*).
Zur Erinnerung: Fakutät von *n* ist das Produkt aller ganzen Zahlen von 1 bis *n*.
Zum Beispiel wäre die Fakultät von vier:  
`4! = 1 * 2 * 3 * 4 = 24`  
Eine Python Funktion dazu könnte man so schreiben:

```python 
def factorial_recursive(n):
    # Base case: 1! = 1
    if n == 1:
        return 1

    # Recursive case: n! = n * (n-1)!
    else:
        return n * factorial_recursive(n-1)


print(factorial_recursive(6))  # => 720
```
Im Allgemeinen ist der Einsatz von rekursiven Funktionen aber eher selten.
Häufig haben rekursive Funktionen das Problem das sie schnell sehr langsam und uneffizient werden.

Deutlich wichtiger (in meinen Augen...) sind dagegen lokale Funktionen.

## Lokale Funktionen
Als Lokale Funktionen bezeichnet man Funktionen die innerhalb einer anderen
Funktion definiert werden.

<!--pytest-codeblocks:skip-->
```python
def outer_function():
    def inner_function():
        pass # hier kommt der Code der inneren Funktion hin

    # hier ist die eigentliche Funktion und die benutzt inner_function
    inner_function()
```

Ein erstes Beispiel einer solchen Funktion (`double_n_times()`) mit lokaler/innerer Funktion (`double_number()`):

```python
def double_n_times(a, n):
    def double_number(number):
        return number * 2

    for _ in range(n):
        a = double_number(a)
    return a

print(double_n_times(5, 10))  # => 5120
```

Und hier ein Beispiel im Bereich String-Handling (Hinweis: `.lower()` ist eine String-Methode und ändert alle Buchstaben in einem String zu Kleinbuchstaben):

```python 
def greeting(name):
    """Prints a greeting that depends on the given name string.
    """
    def is_friend(name):
        friends_list = ["tom", "anna", "erik"]
        return name.lower() in friends_list

    # Print greeting
    if is_friend(name):
        print(f"Hi {name}!!")
    else:
        print(f"Hello {name}.")
```
Eine kleine Nebenbemerkung:  
Beim Programmieren gibt es in der Regel nicht **eine** Lösung für ein Problem, sondern es können oft sehr unterschiedliche Programme für eine und dieselbe Aufgabe geschrieben werden.
Im oberen Beispiel, könnte man die Zeile `return name.lower() in friends_list1` auch problemlos austauschen gegen eine Konstruktion mit `if`.


> ### Mini Quiz: 
> Was wird bei greeting("Anna") ausgegeben:
> a) Hello Anna.  
> b) False  
> c) Hi Anna!!

<!--pytest-codeblocks:cont-->
```python 
greeting("Anna")  # => Hi Anna!!
greeting("Sandra")  # => Hello Sandra.
```
Lokale Funktionen sind nicht direkt aufrufbar. Man verwendet sie daher v.a. für kleine Helferfunktionen (um unnötige Wiederholung zu vermeiden und/oder die Struktur der Hauptfunktion klarer zu machen).

In dem oberen Beispiel spart man damit keinen Code.
Aber es wird vielleicht etwas übersichtlicher.

Bei den Funktionen hatten wir auch schonmal kurz das Thema **Namespaces**.
Dort haben wir gesehen, das Variablen die innerhalb einer Funktion definiert werden (also "lokal"), von aussen nicht sichtbar/nutzbar sind. Das Gleiche gilt auch für lokale Funktionen, diese sind für den Rest eines Programmes nicht sichtbar.

<!--pytest-codeblocks:expect-error-->
```python 
is_friend("Tom")  # => NameError: name 'is_friend' is not defined
```
Wenn die Helfer-Funktion auch in anderen Funktionen benutzt wird, macht es Sinn sie als Helferfunktion in ein eigene Datei zu stecken
und zu importieren, z.B. mit `from helper_functions import is_friend`.


### Lokale Funktionen: Wann benutzen und wann nicht?

Lokale Funktionen machen Sinn als kleine Helfer-Funktionen, wenn sie
- nicht durch andere Funktionen benötigt werden
- mehrere Male in der äusseren Funktion aufgerufen werden und/oder
- die äussere Funktion lesbarer machen

Stellen wir uns vor die Funktion `is_friend` würde nun in zwei weiteren Funktionen eingesetzt. Dann müsste sie in beiden lokal definiert werden. Das ist aber umständlich und sehr fehleranfällig (z.B. wenn eine geändert wird, die andere aber versehentlich nicht). Darum würde man in diesem Fall lieber eine eigene Funktion definieren (oder importieren):
```python 
def is_friend(name):
    """Return True if name is in friends_list
    """
    friends_list = ["tom", "anna", "erik"]
    return name.lower() in friends_list


def greeting(name):
    """Prints a greeting depending on the name string.
    """
    if is_friend(name):
        print(f"Hi {name}!!")
    else:
        print(f"Hello {name}.")
        
        
def goodbye(name):
    """Prints a goodbye depending on the name string.
    """
    if is_friend(name):
        print(f"Bye {name}!!")
    else:
        print(f"Goodbye {name}.")
```

### Funktionen mit variabler Parameter-Anzahl
Alle bisherigen Funktionen die wir angeschaut haben, erwarten eine bestimmte Anzahl an Parametern. Eventuell gibt es einige mit default-Wert die ausgelassen werden können, aber es dürfen auf keinen Fall mehr als die definierten Parameter übergeben werden.

<!--pytest-codeblocks:expect-error-->
```python 
def multiply_all(a, b, c):
    print(a * b * c)

multiply_all(1, 2, 3, 4)  # => TypeError: multiply_all() takes 3 positional arguments but 4 were given
```
Wenn wir die Anzahl aber wie in diesem Fall bewusst offen lassen möchten können wir in Python `\*args` benutzen (kurz für *arguments*).

```python 
def multiply_all(*args):
    product = 1
    for num in args:
        product *= num
    print(product)

multiply_all(1, 2, 3, 4)
```
+ Nur am Rande 1: hier nutzen wir `product *= num` was aber das gleiche ist wie `product = product * num`.  
Es gibt in Python genauso auch `+=`, `-=`, `*=` und `/=`:
```python
a = 5
a += 2  # Gleiche wie a = a + 2 
a -= 2  # Gleiche wie a = a - 2
a *= 2  # Gleiche wie a = a * 2
a /= 2  # Gleiche wie a = a / 2
```

+ Nur am Rande 2: Für solche Operationen gibt es in Python fast immer schon irgendwo eine Funktion die genutzt werden kann. Hier z.B. in der Bibliothek `math` die wir schon gesehen hatten.

```python 
import math
def multiply_all(*args):
    print(math.prod(args))

multiply_all(1, 2, 3, 4)
```

###  \*\*kwargs -> variable Anzahl benannter Parameter
Analog zu `\*args` können mit `\*\*kwargs` auch benannte Parameter in unbestimmter Anzahl an eine Funktion übergeben werden. Dies wird aber eher später von Bedeutung sein wenn wir mit komplexeren Funktionen jonglieren. 
Trotzdem schonmal ein Beispiel dazu:

```python 
def guess_the_animal(**kwargs):
    """Give some hints as keywords-value pairs for guessing an animal.
    """
    print("Hi! Can you guess what animal I mean?")
    for key, value in kwargs.items():
        print(f"{key}: {value}")


guess_the_animal(legs=4, color="gray", weight="5000 kg") 
```
