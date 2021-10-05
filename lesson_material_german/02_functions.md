# Funktionen

Wir haben schon Funktionen benutzt: `print()`, `type()`, aber auch `int()` oder
`str()`.

Diese Funktionen sind bei Python schon "eingebaut".

Die meisten Funktionen die wir nutzen werden sind aber entweder:
- selbst geschrieben (oder: definiert)
- importiert (geladen)


## Definieren von Funktionen
```python 
def gute_sache():
    print("Einmal Aufgeschrieben...")
    print("... 1000x benutzt!")
    print("Wieder 3 Zeilen Code gespart.")
    
```
Wenn wir diesen Code ausführen passiert aber erstmal nix!
Funktionen werden erst beim expliziten Aufrufen ausgeführt:
<!--pytest-codeblocks:cont-->
```python 
gute_sache()
```
Diese Funktion gibt nichts zurück (nur print) und verlangt keine Argumente.

Die allgemeine Struktur von Funktionen ist:
<!--pytest-codeblocks:skip-->
```python 
def function_name(parameter1, parameter2, etc):
    ... code ...
    return result
```
Besser sogar noch so, mit einem sogenannten Docstring der die grundlegende
Dokumentation der Funktion darstellt. Hierin können die Funktion und die Parameter
beschrieben werden. Zusätzlich kann aber auch ein Code Beispiel angegeben werden
oder Refernenzen genannte werden.

<!--pytest-codeblocks:skip-->
```python 
def function_name(parameter1, parameter2, ...):
    """Hier wird die Funktion beschrieben
    """
    ... code ...
    return result
```
Es gibt auch Funktionen ohne parameter (siehe oben `gute_sache()`).
Ausserdem kann man defaults festlegen! Diese werden genutzt, falls kein
neuer Wert für die entsprechenden Parameter eingegeben wird.

<!--pytest-codeblocks:skip-->
```python 
def function_name(p1, p2=3.1415):
    """Hier wird die Funktion beschrieben
    """
    ... code ...
    return result
```
Hier ein anderes Beispiel einer einfachen Funktion mit 2 Parametern:
```python 
def x_vs_y(x, y):
    """Compare the numbers x and y.
    """
    if x == y:
        print("They are equal!")
    elif x > y:
        print(f"{x} is bigger than {y}")
    else:
        print(f"{x} is smaller than {y}")

x_vs_y(10, 70)  # => 10 is smaller than 70

x_vs_y(0.000001, 1e-5)  # => 1e-06 is smaller than 1e-05
```
Aber was nicht funktioniert ist:
<!--pytest-codeblocks:skip-->
```python 
x_vs_y(10) # => TypeError: x_vs_y() missing 1 required positional argument: 'y'
```
### Default Werte
Hier eine Beispielfunktion in der einer der beiden möglichen Parameter
einen Default-Wert erhält:

```python 
def boxes_to_eggs(n_boxes, eggs_per_box=6):
    """Compute the total number of eggs in n_boxes boxes.
    """
    return n_boxes * eggs_per_box

print(boxes_to_eggs(5))  # => 30
print(boxes_to_eggs(5, 10))  # => 50
print(boxes_to_eggs(5, eggs_per_box=10))  # => 50
print(boxes_to_eggs(eggs_per_box=10, n_boxes=5))  # => 50
```
ABER... Folgendes geht nicht, da die benannte Parameter nicht vor unbenannten
an die Funktion übergeben werden dürfe.
<!--pytest-codeblocks:skip-->
```python 
print(boxes_to_eggs(n_boxes=5, 10))  # => SyntaxError: positional argument follows keyword argument
```
### Optional:
Die Frage der Reihenfolge wird bei weiteren möglichen
Parametern noch wichtiger. Versehentliche Vertauschungen können teilweise
zwar zu funktionierendem Code führen, aber nicht zu den gewüschten
Ergebnissen...
```python 
def boxes_to_eggs(n_boxes,
                  eggs_per_box=6,
                  broken_eggs_per_box=0):
    """Compute the total number of eggs in n_boxes boxes.
    """
    return int(n_boxes * (eggs_per_box - broken_eggs_per_box))

print(boxes_to_eggs(5))  # => 30
print(boxes_to_eggs(5, 10))  # => 50
print(boxes_to_eggs(5, eggs_per_box=10))  # => 50

print(boxes_to_eggs(5, broken_eggs_per_box=0.25))  # => 25
print(boxes_to_eggs(5, eggs_per_box=10,
                    broken_eggs_per_box=0.25))  # => 48
print(boxes_to_eggs(5, broken_eggs_per_box=0.25,
                    eggs_per_box=10))  # => 48
```
Aber...
<!--pytest-codeblocks:cont-->
```python 
print(boxes_to_eggs(5, 10, 0.25))  # => 48
print(boxes_to_eggs(5, 0.25, 10))  # => -48 !!!
```
> ### MINI Quiz
> Eine Funktion kann Werte/Daten zurück geben an das Programm über...  
> a) print  
> b) return  
> c) out   
> d) assert
> 
> Eine Funktion ohne "return" statement gibt folgendes zurück...  
> a) Nichts  
> b) die Parameter  
> c) die Variablen  
> d) None


### Namensräume
Unter einem Namensraum versteht man einen Teil (oder Raum) innerhalb eines Programmes, in dem ein Name (z.B. Variablen und Funktionen) gültig ist.
In Python gibt es drei Arten solcher Geltungsbereiche:
+ Lokaler Geltungsbereich (innerhalb einer Funktion oder Methode)
+ Modularer Geltungsbereich (innerhalb einer Datei/eines Programmes)
+ Eingebauter Geltungsbereich (von Python definierte Namen sind immer gültig)

Aber schauen wir das Ganze besser mal an einem Beispiel an: 

```python 
a = 5
b = 7

def do_stuff():
    a = 1000
    #print(a)  # hiermit kann man a innerhalb der Funktion anschauen
    #print(locals())  # hiermit werden die lokalen Variablen gezeigt
    return a + b
    
result = do_stuff()
print(a, b, result)  # => 5 7 1007
```
<!--pytest-codeblocks:cont-->
```python 
def do_stuff():
    # a wird lokal nicht definiert
    #print(a)
    #print(locals())
    return a + b
    
result = do_stuff()
print(a, b, result)  # => 5 7 12
```
Globale Variablen können zwar benutzt, aber i.d.r nicht verändert werden!

<!--pytest-codeblocks:expect-error-->
```python 
def do_stuff():
    a = a * 2
    return a

a = 10
do_stuff() # ==> UnboundLocalError: local variable 'a' referenced before assignment
```
### Optional\*: Hä? Wann wird jetzt was verändert??
\* *Genau. Optional bedeutet auch: nicht Prüfungsrelevant*  
Gerade haben wir gesehen, dass globale Variablen nicht lokal in einer Funktion
geändert werden können. Das gilt aber nur für unveränderbare (inmmutable) Datentypen,
so wie int, float, tuple.
Bei veränderbaren (mutable) Datentypen sieht das anders aus. Zumindest wenn
deren eigene Methoden genuzte werden (also Funktionen die mit `.xzy()` aufgerufen
werden). 
In diesen Fällen wird die Methode direkt auf das Objekt selbst angewandt:

```python 
def do_other_stuff():
    my_list.append(55)

my_list = [1, 2]
do_other_stuff()
print(my_list)  # => [1, 2, 55] 
```
`my_list` wurde also durch `do_other_stuff()` verändert.

Wohingegen Folgendes nicht geht:

<!--pytest-codeblocks:expect-error-->
```python 
def do_other_stuff():
    my_list = my_list + [55]

my_list = [1, 2]
do_other_stuff()
print(my_list)  # UnboundLocalError: local variable 'my_list' referenced before assignment
```
```python 
def do_other_stuff(my_list):
    my_list.append(55)

my_list = [1, 2]
do_other_stuff(my_list)
print(my_list)  # => [1, 2, 55]

```
Selbst wenn `my_list` als Parameter an die Funktion übergeben wird, läuft
zwar der Code, aber wird doch nicht die globale Variable verändert:
```python 
def do_other_stuff(my_list):
    my_list = my_list + [55]
    print(f"inner function: {my_list}")

my_list = [1, 2]
do_other_stuff(my_list)  # => inner function: [1, 2, 55]
print(my_list)  # => [1, 2]
```
Manchmal führt dieses Verhalten zu Schwierigkeiten. Z.B. wenn eine
Funktion versehentlich den Anschein erweckt als würde sie einen neuen
Wert ausgeben, aber dann leider doch auch den EIngabewert selbst verändert.

Hier mal ein Beispiel. Wir wollen eine Funktion haben die alle Werte 
einer Liste verdoppelt.
```python 
def double_values(values):
    for i in range(len(values)):
        values[i] = values[i] * 2
    return values

my_list = [5, 10, 15]
my_list_x2 = double_values(my_list)
print(my_list, my_list_x2)  # => [10, 20, 30] [10, 20, 30]
```
Wenn in Python Code steht der die Form hat `b = my_function(a)`,
dann erwarten wir (zurecht!) das `a` durch den Aufruf nicht verändert wird.

In dem konkreten Fall kann das auf verschiedene Arten erreicht werden.
Entweder eine neue Variable wird in der Funktion angelegt, oder es wird
explizit eine echte Kopie erstellt.

```python 
def double_values(values_input):
    values = values_input.copy()  # eine Kopie wird erstellt
    for i in range(len(values)):
        values[i] = values[i] * 2
    return values
my_list = [5, 10, 15]
my_list_x2 = double_values(my_list)
print(my_list, my_list_x2)  # => [5, 10, 15] [10, 20, 30]
```
oder:
```python 
def double_values(values_input):
    values = []  # eine völlig neue Liste wird angelegt
    for val in values_input:
        values.append(val * 2)
    return values

my_list = [5, 10, 15]
my_list_x2 = double_values(my_list)
print(my_list, my_list_x2)  # => [5, 10, 15] [10, 20, 30]
```
### Vorschau
In der nächsten Vorlesung geht es dann um weitere Typen von Funktionen...
