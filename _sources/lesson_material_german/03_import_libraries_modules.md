# Import aus Bibliotheken/Modulen

Eine der grossen Stärken von Python ist das es sehr viele gute Bibliotheken (libraries) gibt. Diese werden als Bibliotheken oder auch als "Packages" oder "Modules" bezeichnet. Python Bibliotheken sind eigentlich nichts weiter als online verfügbarer Code. Typischerweise enthalten Bibliotheken nützliche Funktionen für bestimmte Zwecke und in der Regel sind Bibliotheken so aufgebaut, dass die darin enthaltenen Funktionen thematisch zusammen passen. Ein Beispiel für eine solche Bibliothek wäre `math`, wie der Name vielleicht schon andeutet eine Bibliothek für mathematische Funktionen.

Sofern eine Bibliothek installiert ist (dazu in der übung mehr) importiert man diese so:

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
from math import sin as sine_function
print(sine_function(1.0))  # => 0.8414709848078965
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
from random import randint

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
> Was wäre dann der korrekte Weg um die Funktion sin zu nutzen?  
> a) math.sin()  
> b) mathezeugs.sin()  
> c) sin()


## Python Universe
Einer der großen Vorteile von Python ist, dass es zu fast allen Themen und Fragen eine Menge an frei verfügbaren Bibliotheken gibt. Bibliotheken für mathematische Funktionen, für das Arbeiten mit großen Datensätzen, das Nutzen von Databases, für Web-searches, für graphische Darstellungen, für das Arbeiten mit Bild, Video, oder Audio-Formaten, für User-Interfaces, für maschinelles Lernen, usw.

Für Anfänger*Innen hat das allerdings auch den kleinen Nachteil, dass man schnell den Überblick verliert.

[xkcd comic zu dem Thema](https://xkcd.com/1987)
![xkcd comic](https://imgs.xkcd.com/comics/python_environment_2x.png)
