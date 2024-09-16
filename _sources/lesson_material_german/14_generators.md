# Generators / Iterators

Wir starten mit einer kurzen Quiz-Frage:

> Wenn ein Pandas DataFrame erstellt wurde (z.B. indem wir eine .csv-Datei importiert haben mit `read_csv()`). Wo sind die Werte des DataFrames dann gespeichert?
>
> (a) Im Ordner in dem ich gerade arbeite.
> (b) In einem temporären Ordner auf der Festplatte.
> (c) Im Arbeitsspeicher.
> (d) Im Prozessor-Speicher (Cache).

Für alle die sich ein wenig mit dem Aufbau eines Rechners auskennen, sicher einfach zu beantworten. Und doch war es bisher beim Programmieren mit Python für uns kein Thema. Für weitere Schritte ist dies aber zunehmend wichtig. Die Daten werden nämlich, wie übrigens alle Variablen die wir im Python Interpreter anlegen, im Arbeitsspeicher hinterlegt. Das ist auch gut und richtig so und war bis hierhin kein Problem um das wir uns Gedanken machen mussten.

Je mehr wir aber in richtig Big Data schauen, umso wichtiger wird dieses Thema werden. Denn der Arbeitsspeicher ist zwar schnell, aber vom Umfang her sehr begrenzt. D.h. es kann in vielen Fällen zu Problemen führen wenn wir zu viele Daten gleichzeitig im Arbeitsspeicher vorhalten wollen.

Eine Möglichkeit damit besser umzugehen, sind sogenannte Generatoren (*generators*). Diese bieten die Möglichkeit immer nur so viele Daten zu laden (oder erzeugen) wie aktuell benötigt. Bevor wir zu den *generators* kommen, starten wir aber mit einer allgemeineren Variante, dem Iterator (*iterator*).

## Iterators

Iteratoren (iterators) sind in Python Objekte, die durch eine Folge von Werten iterieren können. Sie ermöglichen es, über eine Reihe von Elementen in einer for-Schleife zu iterieren, ohne dass die genaue Anzahl der Elemente im Voraus bekannt sein muss. Ein Iterator-Objekt muss das Iterator-Protokoll implementieren, das die **iter**()- und **next**()-Methoden definiert.

Die **iter**()-Methode gibt das Iterator-Objekt selbst zurück und wird automatisch aufgerufen, wenn eine for-Schleife gestartet wird. Die **next**()-Methode gibt das nächste Element der Folge zurück und wird automatisch aufgerufen, wenn die for-Schleife das nächste Element abfragt. Wenn kein weiteres Element vorhanden ist, wird eine StopIteration-Ausnahme ausgelöst.

Ein Beispiel für die Verwendung von Iteratoren ist die Verwendung der built-in-Funktion `iter()` auf einer Liste oder einem Tupel:

```python
numbers = [1, 2, 3, 4, 5]
iterator = iter(numbers)
print(next(iterator)) # 1
print(next(iterator)) # 2
print(next(iterator)) # 3
```

In diesem Beispiel wird die `iter()`-Funktion verwendet, um einen Iterator für die numbers-Liste zu erstellen. Der Iterator wird in der Variablen `iterator` gespeichert. Die `next()`-Funktion wird verwendet, um das nächste  Element im Iterator abzufragen, welches dann ausgegeben wird.

### Was bringt das?

Auf den ersten Blick ist in diesem Beispiel vielleicht nicht unbedingt klar, was das Interator-Konzept hier bringt. Immerhin könnten wir dafür auch einfach direkt eine for-Schleife nehmen:

```python
numbers = [1, 2, 3, 4, 5]
for number in numbers:
    print(number)
```

Ein Iterator ist tatsächlich nicht sinnvoll, wenn nur eine for-Schleife ersetzt werden soll. Er bietet aber die Möglichkeit, nur einzelne Elemente zu entnehmen und merkt sich zudem an welcher Stelle wir stehengeblieben sind. Der richtige Mehrwert von Iteratoren wir v.a. bei den sogenannten Generatoren sichtbar.

## Generators

Generatoren (generators) sind spezielle Iteratoren, die in Python mit  Hilfe der yield-Anweisung erstellt werden. Sie ermöglichen es, einen  Iterator zu erstellen, ohne die gesamte Folge im Speicher zu speichern.  Stattdessen werden nur die Werte erzeugt, die zurzeit benötigt werden.  Dies macht Generatoren besonders nützlich für die Verarbeitung von  großen Datenmengen oder für die Verarbeitung von unendlichen Folgen.

In Python lassen sich Generatoren einfach erstellen und zwar fast genau wie wir bisher auch Funktionen definiert haben, mit der wichtigen Ausnahme dass wir `yield` statt `return` verwenden.
Der Ausdruck `yield` gibt, genau wie return, Werte zurück wenn er im Code erreicht wird. Anders als `return` endet ein Funktionsaufruf allerdings nicht beim `yield` sondern läuft mit dem folgenden Aufruf danach weiter (also z.B. über ein `next()`). Hier ein kurzes Beispiel:

```python
# A simple generator function
def my_generator():
    n = 1
    print('This is printed first')
    # Generator function contains yield statements
    yield n

    n += 1
    print('This is printed second')
    yield n

    n += 1
    print('This is printed at last')
    yield n
    
a = my_generator()
next(a)
```

### Beispiel: city name generator

Stellen wir uns vor, wir wollen sehr viele Stadtnahmen generieren (z.B. für ein Spiel). Dann können wir das auf bisherigem Wege machen über:

```python
import numpy as np

def city_name_generator():
    beginnings = ["Ober", "Unter", "Alt", "Neu", "Klein", "Königs", "Gold"]
    middle = ["strom", "see", "wald", "wies", ""]
    endings = ["hausen", "heim", "ingen", "dorf", "stadt", "bach"]
    return np.random.choice(beginnings) + np.random.choice(middle) + np.random.choice(endings)
    
for _ in range(10):
    print(city_name_generator())
```

Das gibt mir aber immer nur zufällige Kombinationen aus, also möglicherweise auch viele Dopplungen. 

Wie können wir daraus einen echten Python-Generator machen?
Eine Möglichkeit wäre wie folgt:

```python
def city_name_generator():
    beginnings = ["Ober", "Unter", "Alt", "Neu", "Klein", "Königs", "Gold"]
    middle = ["", "strom", "see", "wald", "wies", "", ""]
    endings = ["hausen", "heim", "ingen", "dorf", "stadt", "bach"]
    for a in beginnings:
        for b in middle:
            for c in endings:
                yield a + b + c

city = city_name_generator()
for _ in range(10):
    print(next(city))
```

Hier werden die Stadtnamen immer nur generiert, wenn wir den Generator mit `next()` aufrufen. Und solange bis alle möglichen Kombinationen aufgebraucht sind werden immer neue Namen generiert, ohne Dopplungen!

Was ist aber jetzt, wenn wir alle Stadtnamen auf einmal haben wollen? Das geht mit einer entsprechend langen for-Schleife. Aber auch indem wir mit `list()` erzwingen, dass alle Elemente in eine Liste umgewandelt werden:

```python
# create a new instance of the generator to start with beginning
city = city_name_generator()
all_cities = list(city)
```

### Unendliche Sequenzen

Ein weiterer Anwendungsfall für Generatoren sind unendliche Sequenzen. Hier ein einfaches Beispiel:

```python
def all_seventh():
    n = 0
    while True:
        yield n
        n += 7

seventh = all_seventh()
print(next(seventh))
```

Der Generator geht mit jedem `next()` die Siebenerreihe durch, so lange wir wollen. Es werden dabei aber immer nur die abgefragten Elemente berechnet so dass der Generator hier sehr effizient ist.

