# Objektorientierte Programmierung - Einführung

## Einführung
Bevor wir wirklich loslegen mit der objektorientierten Programmierung (*object-oriented programming*, oft auch einfach nur OOP), eine kurze Wiederholung.

In den vorherigen Kapiteln haben wir uns ein wenig angeschaut, wie man Programme sinnvoll strukturieren kann. Ein zentrales Element hierfür sind **Funktionen**. Damit können ganze Programmteile als eigenständige Sub-Programme abgekapselt werden. Nicht immer wird der Code dadurch direkt viel einfacher, aber es ermöglicht es dem Programme eine logische Struktur zu geben und unterschiedliche Aspekte getrennt voneinander zu bearbeiten oder auch zu verbessern und auszubauen.

**Randnotitz:** Im Zusammenhang mit Funktionen wird manchmal auch von **functional programming** gesprochen ([Funktionale Programmierung](https://de.wikipedia.org/wiki/Funktionale_Programmierung)). Damit wird nicht einfach die Verwendung von Funktionen gemeint (die ist nämlich sowieso eher der Standard), sonder ein spezielles Code-design das einen starken Fokus auf Funktionen legt. Dazu kann z.B. auch gehören, dass dynamisch neue Funktionen generiert werden.

## Objektorientierte Programmierung

Objektorientierte Programmierung (oder "OOP") ist ein Programmierstil oder wird manchmal auch als ein „Programmierparadigma“ bezeichnet um anzugeben, dass es um mehr als kleine Detailfragen geht sondern um eine grundlegende Herangehensweise oder Betrachtungsweise.

Die ursprüngliche Motivation der objektorientierten Programmierung liegt wohl darin begründet, dass das Vorgehen häufig als sehr intuitiv empfunden wird. Das heißt, sobald man sich an die grundlegenden Prinzipien etwas gewöhnt hat...

In der objektorientierten Programmierung wird alles durch **Objekte** beschrieben. Objekte haben Eigenschaften (die **Attribute**) und Fähigkeiten oder Tätigkeiten, die **Methoden**. Wichtig ist, das in Objekten beides zusammenkommt, d.h. Objekte besitzen Attribute und Methoden. Nehmen wir ein einfaches Beispiel: Ein Fahrrad hat bestimmte Eigenschaften, z.B. die Radgröße, die Farbe, die Anzahl der Gänge etc. Das wären die Attribute. Ein Fahrrad hat aber auch Methoden und zwar das Fahren, oder z.B. Klingeln.

![Beispiel einer Klasse Fahrrad](../images/fig_oop_bike_class.png)

Gleich zu Beginn eine wichtige Unterscheidung die am Anfang oft für Verwirrung sorgt: **Klasse vs. Objekt**.
Eine **Klasse** definiert einen Typen, sie ist also eine Art Vorlage. Ein Objekt (oder auch: **Instanz**) ist dagegen nur ein Exemplar das auf einer bestimmten Klasse beruht. Im Beispiel hier wäre *Fahrrad* eine Klasse und jedes echte an uns vorbeifahrende Fahrrad wäre jeweils ein Objekt der Klasse Fahrrad. Keines davon **ist** eine Vorlage für Fahrrad, sondern sie entsprechen der Vorlage Fahrrad weil sie die entsprechenden Eigenschaften und Methoden haben.

Das heißt auch: Es gibt nur **eine Klasse** Fahrrad, aber theoretisch **beliebig viele Objekte** der Klasse Fahrrad.

Verwirrend? Hoffentlich wird das gleich etwas deutlicher.

Übrigens: Neben Funktionen als Möglichkeit zur Strukturierung eines Programmes, haben wir auch schon mit *Methoden* gearbeitet, ohne allerdings genau darauf zu achten was Methoden eigentlich sind. Methoden haben wir bisher einfach als Funktionen kennengelernt, die zu bestimmten Datentypen gehören. Nehmen wir als Beispiel die Listen.

```python
numbers = [2, 5, 11, 3]
print(type(numbers))  # => <class 'list'>
```

Mit `type()` bekommen wir sogar den Hinweis, dass numbers zur Klasse `list` gehört. Wie alles andere in Python auch, ist numbers damit ein Objekt. 

Wir haben gesehen, dass Listen in Python eine ganze Reihe von Methoden mitbringen, z.B:

```python
numbers = [2, 5, 11, 3]
numbers.sort()
print(numbers)
```

Hier ist `.sort()` die Methode. Wir müssen sie nirgendwo her importieren, sie ist einfach bei der Liste "mit dabei". 

Und genauso haben wir gesehn, dass andere Datentypen mit denen wir schon gearbeitet haben, wie integer, float, string, dictionary, set ebenfalls solche Methoden haben. 

### Eine Klasse definieren

Eine Klasse ist nun eine Möglichkeit unsere eigenen Datentypen zu definieren.
Wichtig: Klassen sind absolut nichts, das speziell für Python ist. Die meisten modernen Programmiersprachen erlauben es Klassen zu definieren.

Fangen wir einfach an und definieren einfach mal einen neuen Datentypen, bzw. eine neue Klasse, namens `Point()`:

```python
class Point:
    pass  # pass bedeuted nur, dass nicht passiert

a = Point()
print(a)  # => <__main__.Point object at 0x000001F9DB02BDC0>
```

Mit `class` können wir in Python eine neue Klasse definieren. Im Prinzip können diese genau wie Variablen benannt werden, um aber Variablen, Funktionen und Klassen zu unterscheiden gibt es die Konvention das Klassen im sogenannten *camel case* benannt werden, also jedes Wort mit Großbuchstabe beginnt: `MyClass` oder `DoesWhatYouWant` etc.

Ein Objekt (d.h. eine Instanz einer Klasse) kann danach wie bei Funktionen generiert werden mit, hier war es `a = Point()`. Damit ist `a` ein Objekt der Klasse Point. Auch das können wir übrigens mit `type()` abfragen.

Eine Klasse enthält **Attribute** (attributes) und **Methoden** (methods).
Attribute entsprechen Variablen und -wie oben schon erwähnt- Methoden entsprechen Funktionen.  Beides wird in Python über ein `.`aufgerufen. Wir beginnen mit den Attributen.

### Attribute & Methoden

Attribute können einfach hinzugefügt, bzw. verändert werden. Objekte sind veränderbar, d.h. wir können  auch die Attribute anpassen:

<!-- pytest-codeblocks:cont -->

```python
point1 = Point()
point1.x = 4
point1.y = 3
print(point1.x, point1.y)  # => 4 3

point1.x = 0
print(point1.x, point1.y)  # => 0 3
```

Und jetzt möchten wir eine erste Methode zu `Point`hinzufügen:

```python
class Point:
    def position(self):
        print(self.x, self.y)
        
point1 = Point()
point1.x = 4
point1.y = 3
point1.position()
```

Aber jetzt passiert folgendes:

<!-- pytest-codeblocks:expect-error -->

```python
point2 = Point()
point2.position()  # => AttributeError: 'Point' object has no attribute 'x'
```

`point2` ist eine neue Instanz der Klasse Point. Da die Methode `position()`zur Klasse Point gehört, ist diese natürlich verfügbar, aber die Attribute x und y wurde noch nicht definiert. Darum gibt es hier diese Fehlermeldung.

Eigentlich sollte jedes Point-Objekt auf jeden Fall eine x und y-Position haben! Das ist auch möglich, und zwar über eine "init-Methode", auch Konstruktor (*constructor*) genannt. Das ist eine Methode die sofort beim Erstellen eines Objektes aufgerufen werden. In Python nutzen wir dafür eine Methode die mit  `__init__` benannt wird:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def position(self):
        print(self.x, self.y)

point1 = Point(10, 3)
point1.position()  # => 10 3
```

OK. Kurzes mini-Quiz:

>  ### Quiz!
>
> In Python ist "my name is"... : (alle zutreffenden Antworten auswählen)
>
> A) ... eine Instanz.
> B) ... ein Klasse.
> C) ... ein Objekt.
> D) ... ein Funktion.

und

> Nächste Frage: "my name is" ist...
>
> A) ein Objekt der Klasse 'str' (string)
> B) ein Objekt ohne Klasse
> C) ein Objekt der Klasse 'type'

Weiter geht's... 

Jetzt soll die Klasse Point noch eine weitere Methode bekommen und zwar um den Abstand vom Mittelpunkt (0, 0) zu messen.

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def position(self):
        print(self.x, self.y)

    def center_distance(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)

a = Point(4, 3)
print(a.center_distance())  # => 5.0
```

### Eine Klasse, unendlich viele Objekte

Es können beliebig viele Objekte einer Klasse erzeugt werden. Die Klasse bleibt dabei immer dieselbe. Und alle Objekte der Klasse (also alle Objekte die mit Hilfe derselben Klasse erzeugt wurden) verfügen über die gleichen Methoden.

<!-- pytest-codeblocks:cont -->

```python
from random import randint

for _ in range(10):
    new_point = Point(randint(-10, 10), randint(-10, 10))
    point_collection.append(new_point)

for point in point_collection:
    point.position()
```

Die Liste `point_collection` enthält anschließend 10 `Point` Objekte, die zwar alle die gleichen Methoden haben und damit auch alle über die Attribute x und y verfügen, aber die alle ganz unterschiedliche Werte für x und y haben können.



### Objekte die andere Objekte enthalten ("Komposition")

Wie gerade eben gesehen, können wir beliebig viele Instanzen einer Klasse erzeugen (=Objekte). Häufig werden verschiedene Klassen für verschiedene Aufgaben oder Datentypen definiert. Diese können aber in vielfältiger weise miteinander interagieren.

Eine Möglichkeit besteht darin, dass eine Klasse auf weitere Klassen zugreift. Stellen wir uns z.B. vor wir wollen eine Klasse Dreieck (oder auf Englisch: Triangle) erstellen. Dann könnte diese Klasse als Ecken unsere oben definierte `Point` Klasse verwenden.

<!-- pytest-codeblocks:skip -->

```python
class Triangle:
    def __init__(self, corners):
        self.corners = dict()
        for i, (x, y) in enumerate(corners):
            self.corners[i+1] = Point(x, y)

tri_1 = Triangle([(0, 0), (15.5, 7), (-1, -2)])
tri_1.corners[2].position()  # --> 15.5 7
```



### Methoden die Objekte nutzen

Genau wie bei `__init__`, oder auch genau wie bei Funktionen, können Methoden beliebige Argumente nutzen (nicht nur `self`). Das können beliebige Datentypen sein wie List, Set, Dictionary etc. oder auch Objekte von uns selbst definierten Klassen. Es können sogar auch Objekte des gleichen Typs als Argumente verwendet werden. Zum Beispiel wenn wir den Abstand von einem `Point`-Objekt zu einem anderen `Point`-Objekt berechnen wollen:

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def position(self):
        print(self.x, self.y)

    def center_distance(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)
    
    def distance_to_point(self, point):
        dist_x = self.x - point.x
        dist_y = self.y - point.y
        return math.sqrt(dist_x ** 2 + dist_y ** 2)

a = Point(4, 3)
b = Point(-7, 8)
print(a.distance_to_point(b))  # => 12.083045973594572
```

Nochmal ein anderes Beispiel in dem wir eine Klasse `Fruit` definieren. Danach erstellen wir damit verschiedene `Fruit`-Objekte.

<!-- pytest-codeblocks:cont -->

```python
class Fruit:
    def __init__(self, name,
                 good_for=[]):
        self.name = name
        self.good_for = good_for

    def juice(self):
        if "juice" in self.good_for:
            print(f"Voilà! {self.name} juice!")
        else:
            print(f"{self.name} juice? Better not.")


apple = Fruit("apple", good_for=["juice", "cake"])
apple.juice()

banana = Fruit("banana", good_for=["cake"])
banana.juice()
```

Super. 

Wir können jetzt eigene Klassen definieren. 

So what!? Was bringt das denn jetzt überhaupt?

Eine ganze Reihe! Zum einen helfen Klassen um Programme sinnvoll zu strukturieren, denn mit Klassen können wir Objekte so entwerfen, dass sie genau die gewüschten Eigenschaften haben und mit allen nötigen Methoden ausgestattet sind. In unserem ersten Beispiel haben wir etwa dafür gesorgt, dass alle `Point` Objekte eine x-y position mitbringen. Und für alle können wir ohne jeglichen extra import die entsprechenden Methoden nutzen, hier war es `center_distance()`.

