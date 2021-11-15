## Object-oriented programming

Bevor wir wirklich loslegen mit dem object-oriented programming, eine kurze Wiederholung.

Letzte Woche (und teilweise auch davor) haben wir uns ein wenig angeschaut, wie man Programme sinnvoll strukturieren kann. Ein zentrales Element hierfür sind **Funktionen**. Damit können ganze Programmteile als eigenständige Sub-Programme angekapselt werden. Nicht immer wird der Code dadurch direkt viel einfacher, aber es ermöglicht es dem Programme eine logische Struktur zu geben und unterschiedliche Aspekte getrennt voneinander zu bearbeiten oder auch zu verbessern und auszubauen.

**Randnotitz:** Im Zusammenhang mit Funktionen wird manchmal auch von **functional programming** gesprochen ([Funktionale Programmierung](https://de.wikipedia.org/wiki/Funktionale_Programmierung)). Damit wird nicht einfach die Verwendung von Funktionen gemeint (die ist nämlich sowieso eher der Standard), sonder ein spezielles Code-design das einen starken Fokus auf Funktionen legt. Dazu kann z.B. auch gehören, dass dynamisch neue Funktionen generiert werden.

Neben Funktionen als Möglichkeit zur Strukturierung eines Programmes, haben wir auch schon mit *Methoden* gearbeitet, ohne allerdings genau darauf zu achten was Methoden eigentlich sind. Methoden haben wir bisher einfach als Funktionen kennengelernt, die zu bestimmten Datentypen gehören. Nehmen wir als Beispiel die Listen.

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

Eine Klasse ist nun eine Möglichkeit unsere eigenen Datentypen zu definieren!
Wichtig: Klassen sind absolut nichts, das speziell für Python ist. Die meisten modernen Programmiersprachen erlauben es Klassen zu definieren.

Eine wichtige Unterscheidung dabei ist: Klassen vs. Objekt.
Eine **Klasse** definiert einen Typen. Ein Objekt ist eine Instanz einer Klasse. Also: der Datentyp List ist eine Klasse, die gerade verwendete Instanz davon (`numbers`) ist dann ein **Objekt**. Verwirrend? 
Hoffentlich wird das gleich etwas deutlicher.

Fangen wir einfach an und definieren wir einen neuen Datentypen, eine neue Klasse namens `Point()`:

```python
class Point:
    pass  # pass bedeuted nur, dass nicht passiert

a = Point()
print(a)  # => <__main__.Point object at 0x000001F9DB02BDC0>
```

Mit `class` können wir in Python eine neue Klasse definieren. Im Prinzip können diese genau wie Variablen benannt werden, um aber Variablen, Funktionen und Klassen zu unterscheiden gibt es die Konvention das Klassen im sogenannten *camel case* benannt werden, also jedes Wort mit Großbuchstabe beginnt: `MyClass` oder `DoesWhatYouWants` etc.

Ein Objekt (d.h. eine Instanz einer Klasse) kann danach wie bei Funktionen generiert werden mit, hier war es `a = Point()`. Damit ist `a` ein Objekt der Klasse Point. 

### Was kann die Klasse?

Eine Klasse enthält **Attribute** (attributes) und **Methoden** (methods).
Attribute entsprechen Variablen und -wie oben schon erwähnt- Methoden entsprechen Funktionen.  Beides wird in Python über ein `.`aufgerufen.
Attribute können einfach hinzugefügt, bzw. verändert werden:

<!-- pytest-codeblocks:cont -->

```python
point1 = Point()
point1.x = 4
point1.y = 3
print(point1.x, point1.y)  # => 4 3
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

Eigentlich sollte jedes Point-Objekt auf jeden Fall eine x und y-Position haben! Das ist auch möglich, und zwar über sogenannte Konstuktoren (*constructors*), das sind Methoden die beim Erstellen eines Objektes aufgerufen werden. In Python nutzen wir dafür eine Methode die mit  `__init__` benannt wird:

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

>  ### Mini Quiz
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

### Methoden die Objekte nutzen

Methoden können auch Objekte des gleichen Typs als Argumente nehmen. Zum Beispiel wenn wir den Abstand von einem `Point`-Objekt zu einem anderen `Point`-Objekt berechnen wollen:

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

## Und jetzt zu den Superkräften!

Es gibt aber noch viele andere Möglichkeiten mit Klassen. Eine besonders mächtige ist die sogenannte "Vererbung".

### Vererbung (inheritance)

Klassen können anderen Klassen ihre Eigenschaften vererben. Das ist in der Praxis tatsächlich oft sehr nützlich! Hier mal ein Beispiel: Wir haben gerade Punkte definiert (`Point`-Klasse) und möchten jetzt auch andere geometrische Typen entwerfen. Dann müssen wir nicht unbedingt alles neu definieren, sondern es geht auch das Folgende:

<!-- pytest-codeblocks:cont -->

```python
class Circle(Point):
    def __init__(self, x, y, radius):
        self.x = x
        self.y = y
        self.radius = radius

a = Circle(11, 2, 5)
print(a.center_distance())  # => 11.180339887498949
```

Ohne das wir eine Methode neu definiert haben, funktioniert schon der Aufruf `.center_distance()`!

Das liegt daran, dass wir hier gesagt haben das `Circle` auf die Klasse `Point`zurückgreifen darf. Man spricht hier von "Vererbung" und demensprechend auch davon, dass `Circle` das Kind (*child*) ist von `Point`. 

`Point` wird die "Basisklasse" oder auch Super-, Ober- oder Elternklasse genannt (*parent class*), während `Circle` in diesem Beispiel die "abgeleitete Klasse" oder auch Sub-, Unter-, oder Kindklasse heißt (*child class*).

