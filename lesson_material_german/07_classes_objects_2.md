# Classes - part 2



## Wiederholung (mit kleinen Extras)

Letzte Woche haben wir Klassen und die damit erzeugten Objekte kennengelernt, bzw. wie wir Klassen in Python definieren können. Klassen können Attribute (~Variablen) und Methoden (~Funktionen) haben. Das heißt, das Objekte sowohl Daten als auch dazugehörige Funktionen beinhalten. Wir hatten das letzte Mal eine Klasse `Point` und alle Objekte dieser Klasse haben nicht nur eine x-y Koordinate, sondern sie bringen auch einige Methoden mit, z.B. eine Methode zur Bestimmung des Abstandes zum Ursprung (0,0).

Dann haben wir gesehen wie einfach eine neue Klasse definiert werden kann, die alle Eigenschaften und Methoden einer anderen Klasse *erbt*. Man nennt solch eine Klasse *child class* (oder abgeleitete Klasse) und den Prozess *Vererbung* (*inheritance*).

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def center_distance(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)

a = Point(-7, 8)
print(a.center_distance())  # => 10.63014581273465


# Neue Klasse: Circle
class Circle(Point):
    def __init__(self, x, y, radius):
        self.x = x
        self.y = y
        self.radius = radius

a = Circle(11, 2, 5)
print(a.center_distance())
```

Es gibt auch noch eine andere Möglichkeit den Konstruktor (`__init__()`) anzupassen und gleichzeitig auf den Konstruktor der *parent class* zuzugreifen und zwar mit `super()`:

<!-- pytest-codeblocks:cont -->

```python
class Circle(Point):
    def __init__(self, x, y, radius):
        super().__init__(x, y)  # damit wird der Konstruktor der parent class ausgeführt
        self.radius = radius

a = Circle(11, 2, 5)
print(a.center_distance())
```

V.a. wenn der Konstruktor einer Klasse viele Parameter setzt und/oder weitere Schritte enthält macht der Einsatz von `super()`an dieser Stelle den Code einfacher.



## Wiederholung aus der Übung

In der Übung diese Woche haben wir angefangen mit Klassen zu programmieren. Dabei kamen einige Schwierigkeiten zutage, v.a. da das Programmieren mit Klassen sich sehr von linearen (prozeduralen) Skripten unterscheidet, aber eben auch vom Umfang mit Funktionen mit denen wir schön öfter zu tun hatten.

- Über `class XYZ:` wird eine abstrakte Klasse *definiert*. Der eigentliche Code darin wird aber erst ausführbar, wenn ein Objekt der Klasse erzeugt wird (`a = XZY()`).
  Attribute die erst im Konstruktor (`__init__()`) gesetzt werden, werden erst erzeugt wenn auch ein Objekt der Klasse erzeugt wird.
  <!-- pytest-codeblocks:expect-error -->

  ```python
  class DoNothing:
      def __init__(self):
          self.chill = True
  
  print(DoNothing.chill)  # => AttributeError: type object 'DoNothing' has no attribute 'chill'
  ```

  Das geht also nicht, sondern erst wenn ein Objekt erstellt wird:
  <!-- pytest-codeblocks:cont -->

  ```python
  a = DoNothing()
  print(a.chill) # => True
  ```

  

- Eine Methode einer Klasse wird zwar wie eine Funktion definiert (mit `def abc(): ...` ), aber die Methode ist nicht global verfügbar. Nur über erzeugte Objekte der Klasse können die Methoden ausgeführt werden. Beispiel:

  <!-- pytest-codeblocks:expect-error -->

  ```python
  class SuperPrint:
      def upper_print(self, text):
          print(text.upper())
  
  upper_print("mein text soll grösser werden")  # => NameError: name 'upper_print' is not defined
  ```

  Erst wenn ein Objekt der Klasse SuperPrint erzeugt wird, kann damit die Methode `upper_print()` ausgeführt werden. Die Methode gehört also zu allen Objekten der Klasse `SuperPrint` und ist ansonsten nicht verfügbar!

  <!-- pytest-codeblocks:cont -->

  ```python
  printer = SuperPrint()
  printer.upper_print("mein text soll grösser werden")
  ```

  

Ein weiterer wichtiger Punkt den wir in der Übung angefangen haben, war ein Gefühl dafür zu bekommen wann, warum, und wie man Klassen überhaupt einsetzt. Als Beispiel dafür haben wir angefangen ein erstes kleines Spiel zu programmieren: Pig (oder "Böse Eins"). Die Spielregeln sind einfach:

Spielregeln:

* 2 Spieler*Innen spielen gegeneinander und wechseln sich ab. Beide starten mit 0 Punkten.
* Spieler\*In der/die an der Reihe ist würfelt mit 1 Würfel (Zufallszahl -> 1-6). Die Ergebnisse werden zusammengezählt, bis Spieler\*In beschließt zu stoppen, oder: bis eine Eins gewürfelt wird. Wurde gestoppt ohne das eine Eins kam, dann werden die Punkte alle dazugezählt. Wurde eine Eins gewürfelt,  gibt es keine Punkte dazu.
* Wer als erstes 100 Punkte erreicht, gewinnt.

Um dieses Spiel objektorientiert zu programmieren, muss erst überlegt werden welche Klassen es denn geben soll/muss. Typischerweise orientiert man sich bei der Suche danach an den wichtigsten Gegenständen/Elementen/Akteuren. In diesem Beispiel würden z.B. folgende Klassen Sinn machen:

* Würfel --> class Dice
* Spieler*In --> class Player
* Spiel/Spielregeln/Spielablauf/Spielmechanik --> class PigGame

Als nächstes muss geschaut werden, welche Eigenschaften (Attribute) und Handlungen/Möglichkeiten (Methoden) die Klassen bräuchten. Am einfachsten dabei ist die Klasse `Dice`, den die muss v.a. würfeln können was ja nichts anderes ist als Zufallszahlen von 1 bis 6 zu liefern.

```python
from random import randint


class Dice:
    def __init__(self, sides=6):
        self.sides = sides

    def roll(self):
        return randint(1, self.sides)
```

Wie oben bereits erwähnt, macht diese Klasse aber erstmal nichts. Dazu muss erst ein `Dice`-Objekt ereugt werden.

<!-- pytest-codeblocks:cont -->

```python
dice = Dice()
print(dice.roll())
```

Die zweite Klasse ist dann `Player` und das wird schon deutlich schwieriger!

```python
class Player:
    def __init__(self, name):
        self.name = name
        self.score = 0
        
    def make_move(self, dice):
        additional_score = 0
        dice_result = dice.roll()
        if dice_result == 1:
            additional_score = 0
            print(f"{self.name} got a 1.")
        else:
            additional_score += dice_result
            print(f"{self.name} rolled a {dice_result}.")
            print(f"Total score of this round is {additional_score}.")
        # ... finish code next week
```

Jede(r) Spieler*In hat zwei Eigenschaften: einen Namen (`.name`) und einen Punktestand (`.score`). Dazu kommt eine Methode um einen Spielzug zu machen (`.make_move()`). Diese Methode haben wir in der Übung bisher nur skizziert und werden sie erst nächste Woche fertigstellen. Aber ein sehr wichtiges Element vom Objektorientiertem Ansatz kann man an dieser Stelle schon gut sehen: Objekte werden an andere Objekte weiter gegeben. Es muss also erstmal ein Würfel-Objekt erstellt werden damit ein `Player`-Objekt spielen kann!

<!-- pytest-codeblocks:skip -->

```python
player1 = Player("Helmut")
player2 = Player("Sandra")
dice = Dice()

player1.make_move(dice)
player2.make_move(dice)
```

Also: Genauso wie ich in einem echten Würfelspiel den Würfel (ein Objekt) an eine(n) Spieler*In geben würde, wird auch hier das Objekt `Dice` an `Player` gegeben. Und zwar nicht die als feste Eigenschaft (also: Attribut), sondern nur als Argument in der Methode `.make_move()`. Vielleicht erscheint das jetzt erstmal etwas seltsam. Immerhin ist ein Würfelwurf auch nichts besonders kompliziertes und ich könnte ja auch einfach eine Funktion dafür schreiben oder sogar nur direkt `random.randint(1, 6)` aufrufen. 

Der Hauptvorteil ist, dass klar strukturiert ist welche Eigenschaften wo festgelegt werden. Nämlich immer nur bei den dazugehörigen Objekten. Sprich: Wenn `player1` dran ist, geht der Würfel an die entprechende `make_move()`Methode, usw.

Wenn das Spiel jetzt mit einem anderen Würfel stattfinden soll, muss nur der Würfel verändert werden, z.B. so:

<!-- pytest-codeblocks:skip -->

```python
player1 = Player("Helmut")
player2 = Player("Sandra")
dice = Dice(sides=4)

player1.make_move(dice)
player2.make_move(dice)
```

Weiter mit dem Spiel geht's dann in der nächsten Übung!

Jetzt kommen wir zu noch ein paar weiteren Eigenschaften und Möglichkeiten der Objektorientierten Programmierung.

## Datenkapselung

Datenkapselung ist im Prinzip nichts anderes als der Schutz von Daten (Attributen) vor unmittelbarem Zugriff. Wir haben schon gesehen, dass Attribute auch einfach eingesehen und geändert werden können, z.B. `point1.x = 5`. Aber *idealerweise* versuchen die meisten Programmieren*Innen ihre Klassen so zu designen, dass Attribute v.a. über sogennante "Getter" und "Setter"-Methoden aufgerufen werden. 

### Public -> Protected -> Private

Eine wichtige Eigenschaft von Klassen, auch im Zusammenhang mit der Datenabstraktion, ist die Möglichkeit die Zugriffsrechte auf die Attribute festzulegen. Python ist hierbei deutlich weniger strikt als viele andere Programmiersprachen, d.h. wir erstellen jetzt gleich keine high-security Programme. Aber es gibt auch in Python die wichtige Unterscheidung zwischen frei zugänglichen (public), geschützten (protected) und unzugänglichen (private) Attributen.

Python hat hier also drei Stufen:

• name (**Public**): 
 Attribut ohne führende Unterstriche; sind innerhalb einer Klasse und auch von außen les- und schreibbar.

• _name (**Protected**): 
 Man kann zwar von außen lesend und schreibend zugreifen, der/die Entwickler*In macht aber klar das man diese Attribute so nicht benutzen sollte; Protected-Attribute sind beim Importieren wichtig.

• __name (**Private**): 
 Sind von außen weder sichtbar noch benutzbar 

<!-- pytest-codeblocks:expect-error -->

```python
class MagicSpell:
    name = "Expecto Patronum"
    _book = "Magic Spells IV"
    __explanation = "Here is how it works..."

spell = MagicSpell()

print(spell.name)  # => Expecto Patronum
print(spell._book)  # => Magic Spells IV
print(spell.__explanation)  # => AttributeError: 'Spell' object has no attribute '__explanation'

```

Hier ist also zu sehen, dass die *protected attributes*  durchaus noch ganz normal eingesehen werden können! Bei Python ist der eine Unterstrich vor einem Attributsname eher als ein Hinweis an Nutzer\*Innen und v.a. Entwickler\*Innen zu verstehen, diese Variablen nicht einfach zu verändern. Das entspricht in etwa einem "Zutritt auf eigene Gefahr"-Schild.

Die Attribute mit Status *private* geben aber einen `AttributeError`, d.h. diese können tatsächlich nicht mehr wie zuvor aufgerufen und verändert werden.

Das gleiche geht natürlich auch mit einem Konstruktor:

<!-- pytest-codeblocks:expect-error -->

```python
class MagicSpell:
    def __init__(self, name, book, explanation):
        self.name = name
        self._book = book
        self.__explanation = explanation

spell = MagicSpell("Expecto Patronum",
                   "Magic Spells IV",
                   "Here is how it works...")

print(spell.name)  # => Expecto Patronum
print(spell._book)  # => Magic Spells IV
print(spell.__explanation)  # => AttributeError
```



Bevor jemand jetzt aber auf die Idee kommt, damit sensible Daten zu schützen: Einen richtig strengen Schutz der Attribute gibt es bei Python nicht! Der Zugang wird nur bewusst erschwert. Theoretisch können die verstecken Attribute aber noch immer eingesehen werden mit:

<!-- pytest-codeblocks:cont -->

```python
print(spell._MagicSpell__explanation)  # nur zur Info, bitte diesen Weg nicht in Programmen nutzen!
```

Oder, noch einfacher, einfach mal im "Variable Explorer" in Spyder öffnen...

Als kurzes Beispiel einmal den Fall, dass wir nicht wollen das Nutzer\*Innen .x oder .y eines `Point`-Objekts einfach ändern können:

```python
class Point:
    def __init__(self, x, y):
        self.__x = x
        self.__y = y

    def get_position(self):
        return self.__x, self.__y

    def set_position(self, x, y):
        self.__x = x
        self.__y = y

a = Point(10, 3)
print(a.get_position())

a.set_position(8, 4)
print(a.get_position())

# a.x  # => AttributeError
```

Da man die Attribute mit Status nicht nur nicht einsehen kann, sondern diese auch nicht einmal als vorhanden angezeigt werden spricht man hier auch vom **Geheimnisprinzip** einem anderen Konzept im OOP. 

Ok. Schön. Aber wozu das Ganze?

Die (oder zumindest eine) Idee dahinter ist, dass Entwickler*Innen die entwickelten Klassen für bestimmte Nutzungen vorgesehen haben und zum einen "falsche" Nutzung vermeiden möchten, zum Anderen aber auch eine Nutzung nicht unnötig erschweren wollen indem zu viele Details über die genaue Implementierung sichtbar sind.

Oder anders: Wenn wir eine Python Klasse nutzen, interessiert uns meistens vor allem **wie** sie genutzen werden kann und weniger wie der Code dahinter aussieht. Ein Beispiel: Wenn wir eine Liste erstellen, z.B. mit `my_list = [3, 5, 7, 1, 4]` und diese dann sortieren wollen, dann reicht es erstmal völlig wenn wir wissen, dass es dafür eine passende Methode namens `.sort()` gibt (und wie wir diese benutzen). Welche Unterfunktionen etc. dahinter stecken ist erstmal nicht unsere Hauptsorge.

Naja, aber manchmal ist ein Blick unter die Motorhaube doch ganz nett...

## Magic methods

Magic methods oder auch **Dunder** (von **d**ouble **under**), heißen nicht nur *magic*, sie wirken am Anfang wohl auch ein wenig so. Denn zum einen haben hier alle (zumindest von mir) wahrscheinlich noch nie etwas davon gehört und zum Anderen können Experimente damit sehr seltsame Konsequenzen haben.

Es kann z.B. passieren, dass bestimmte Variablen nicht machen was sie *sollen*, vielleicht so:

```
>>> a
5
>>> a + 7
12.123456
>>> a - 1
4.1010101
```

Was passiert hier? 

Python kaputt?

Irgendwelche Ideen?

...

...

... 

OK, vielleicht erstmal etwas Anderes: Was sind Dunder/magic methods jetzt eigentlich?

Das sind Methoden deren Namen mit zwei Unterstrichen umgeben sind. Sie sind nicht für direkte Aufrufe bestimmt sondern werden von bestimmten Aktionen ausgelöst, z.B. auch Operatoren. So ist z.B. `__add__()` die Methode die bei Benutzung von `+` augerufen wird:

```python
a = 5
print(a.__add__(7))  # => 12
print(a + 7)  # => 12
```

Beide Möglichkeiten sind hierbei identisch, mit der Ausnahme dass das direkte Verwenden von `.__add__()` nicht so vorgesehen ist.

Es gibt wirklich **sehr viele** solcher Methoden (siehe z.B. [tutorialstecher.com](https://www.tutorialsteacher.com/python/magic-methods-in-python))! Einige Beispiele die wir (ohne es zu wissen) schon selbst benutzt haben sind:

+ `__add__()` für `+`
+ `__sub__()` für `-`
+ `__mul__()` für `*`
+ `__truediv__()` für `/`
+ `__eq__()` für `==`

Aber auch Methoden die bei der Typen-Umwandlung genutzt werden, z.B.:

+ `__str__()` für `str()`
+ `__float__()` für `float()`



Jetzt vielleicht eine Idee, was in dem *seltsamen* Code-Beispiel oben passiert sein könnte?

Hinweis: Kurz mit `type()` testen was `a` eigentlich ist!

```
>>> type(a)
<class '__main__.FunkyNumber'>
```

**Tadaa!**

`a` ist gar kein `int` wie vielleicht gedacht, sondern ein Objekt der Klasse `FunkyNumber`:

```python
class FunkyNumber(int):
    def __init__(self, value):
        self.value = value

    def __add__(self, x):
        return self.value + x + 0.123456

    def __sub__(self, x):
        return self.value - x + 0.1010101
    
a = FunkyNumber(5)
print(a + 7)
print(a - 1)
print(a * 7)
```

Super funky. Aber was kann das Ganze noch (außer Verwirrung stiften natürlich) ?

Es gibt tatsächlich ziemlich viele sinnvolle Verwendungen für *magic methods* beim objektorientierten Programmieren in Python. Ein Beispiel können wir uns anschauen indem wir wieder zurück zu unserem Standardbeispiel mit der Klasse `Point` gehen. Den manche Dinge funktionieren dort eventuell nicht so wie wir es uns wünschen würden und *magic methods* können da helfen.

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def center_distance(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)
    
a = Point(10, 3)
b = Point(10, 3)
print(a == b)  # => False
```

Was wir hier sehen ist, dass zwei Punkte von Python nicht als gleich (`==`) angesehen werden obwohl sie exakt die gleichen Koordinaten haben. Der Grund ist der, das Python erstmal davon ausgeht, das zwei Objekte nur gleich sind wenn sie auch auf das selbe Objekt verweisen. Sprich: 

<!-- pytest-codeblocks:cont -->

```Python
a = Point(10, 3)
b = a
print(a == b)  # => True
```

Es würde aber (je nach Anwendung) auch Sinn machen, dass zwei Punkte als gleich zu verstehen sind falls ihre Koordinaten gleich sind. Mit *magic methods* können wir das erreichen.

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
    def __eq__(self, p):
        return (self.x == p.x) and (self.y == p.y)

    def center_distance(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)
    
a = Point(10, 3)
b = Point(10, 3)
print(a == b)  # => True
```

Übrigens: auch innerhalb der Standard-Datentypen in Python findet sich die ein oder andere "kreative" Lösung. Ein gutes Beispiel dafür ist die Multiplikation bei Strings, also das`5 * "-"`  möglich ist und `"-----"` ergibt, ist ebenfalls etwas das mit *magic methods* eben einfach definiert werden kann.

