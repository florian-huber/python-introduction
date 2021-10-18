# Import aus Bibliotheken/Modulen

Eine der grossen Stärken von Python ist das es sehr viele gute Bibliotheken (libraries) oder auch "Packages" oder "Modules" gibt. 
Sofern diese installiert sind (dazu in der übung mehr) importiert man diese so:

```python
import math
print(f"pi ist {math.pi}")

print(f"Sinus von pi/2 ist {math.sin(math.pi/2)}")
```

Alternativ können aber auch nur Teile einer Bibliothek importiert werden.
```python
from math import pi  # nur einen Teil importieren!
print(pi)  # => 3.141592653589793
```

Beim Importieren können wenn nötig auch die Namen geändert werden:
```python
from math import sin as sinus_function
print(sinus_function(1.0))  # => 0.8414709848078965
```

## Python universe
Für Python gibt eine Unzahl von Bibliotheken. Einige davon werden wir im Laufe des Semesters einsetzen. Gerade haben wir schon kurz `math` gesehen, eine weitere häufig genutzte Bibliothek ist `random` zur Erzeugung von **Zufallszahlen**.

```python
from random import randint

print(randint(0, 10))
```

oder:
```python
from random import randint

for _ in range(10):
    print(randint(0, 10))
```

## Eigene/lokale Funktionen importieren
Es ist ebenso möglich eigene Funktionen aus `.py` Dateien zu importieren.

Wenn wir z.B. folgende Funktion schreiben:
```python
def dices(n_dices):
    """Returns n_dices random dice results (dice with 1 to 6).
    """
    dice_results = []
    for _ in range(n_dices):
        dice_results.append(randint(1, 6))
    return dice_results
```
und diese in einer Datei `dice_functions.py` speichern, dann können wir die Funktion importieren über (wenn wir uns im selben Ordner befinden!):

<!-- pytest-codeblocks:skip -->
```python
from dice_functions import dices

print(dices(10))
```


### > Mini-Quiz:
> Es ist auch möglich Bibliotheken unter einem gewünschten Namen zu importieren:  
`import math as mathezeugs`  
> Was wäre dann der korrekte weg um die function sin zu nutzen?  
> a) math.sin()  
> b) mathezeugs.sin()  
> c) sin()
    