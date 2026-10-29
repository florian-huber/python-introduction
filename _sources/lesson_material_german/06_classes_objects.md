# Objektorientierte Programmierung - Einführung

## Einführung

Bevor wir wirklich loslegen mit der objektorientierten Programmierung (*object-oriented programming*, oft auch einfach nur OOP), eine kurze Wiederholung.

In den vorherigen Kapiteln haben wir uns ein wenig angeschaut, wie man Programme sinnvoll strukturieren kann. Ein zentrales Element hierfür sind **Funktionen**. Damit können ganze Programmteile als eigenständige Sub-Programme abgekapselt werden. Nicht immer wird der Code dadurch direkt viel einfacher, aber es ermöglicht es dem Programme eine logische Struktur zu geben und unterschiedliche Aspekte getrennt voneinander zu bearbeiten oder auch zu verbessern und auszubauen.

**Randnotitz:** Im Zusammenhang mit Funktionen wird manchmal auch von **functional programming** gesprochen ([Funktionale Programmierung](https://de.wikipedia.org/wiki/Funktionale_Programmierung)). Damit wird nicht einfach die Verwendung von Funktionen gemeint (die ist nämlich sowieso eher der Standard), sonder ein spezielles Code-design das einen starken Fokus auf Funktionen legt. Dazu kann z.B. auch gehören, dass dynamisch neue Funktionen generiert werden.

## Objektorientierte Programmierung

Objektorientierte Programmierung (oder "OOP") ist ein Programmierstil oder wird manchmal auch als ein „Programmierparadigma“ bezeichnet um anzugeben, dass es um mehr als kleine Detailfragen geht sondern um eine grundlegende Herangehensweise oder Betrachtungsweise.

Die ursprüngliche Motivation der objektorientierten Programmierung liegt wohl darin begründet, dass das Vorgehen häufig als sehr intuitiv empfunden wird. Das heißt, sobald man sich an die grundlegenden Prinzipien etwas gewöhnt hat...

In der objektorientierten Programmierung wird alles durch **Objekte** beschrieben. Objekte haben Daten oder Parameter (die **Attribute**) und Fähigkeiten (die **Methoden**). Wichtig ist, das in Objekten beides zusammenkommt, d.h. Objekte besitzen Attribute und Methoden. Nehmen wir ein einfaches Beispiel: Ein Fahrrad hat bestimmte Parameter, z.B. die Radgröße, die Farbe, die Anzahl der Gänge etc. Das wären die Attribute. Ein Fahrrad hat aber auch Methoden und zwar das Fahren, oder z.B. Klingeln.

![Beispiel einer Klasse Fahrrad](C:/Users/flori/images/fig_oop_bike_class.png)

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

Als durchgängiges Beispiel verwenden wir einen **Aufzug**. Ein Aufzug passt gut zu objektorientierter Programmierung, weil er einerseits einen **Zustand** besitzt (z.B. aktuelle Etage und Türstatus) und andererseits bestimmte **Aktionen** ausführen kann (z.B. Tür öffnen oder in eine andere Etage fahren).

Wir beginnen trotzdem ganz klein:

```python
class Elevator:
    pass  # pass bedeutet hier: Dieser Block macht noch nichts.
```

Mit `class` können wir in Python eine neue Klasse definieren. Für Klassennamen ist in Python die **CapWords-/PascalCase-Konvention** üblich: Jedes Wort beginnt mit einem Großbuchstaben, z.B. `Elevator`, `MyClass` oder `BuildingDisplay`.

Die Klasse ist zunächst nur der **Bauplan**. Daraus können wir jetzt konkrete Objekte (Instanzen) erzeugen:

<!-- pytest-codeblocks:cont -->

```python
elevator_a = Elevator()
elevator_b = Elevator()

print(type(elevator_a))  # => <class '__main__.Elevator'>
print(elevator_a is elevator_b)  # => False
```

Beide Objekte wurden mit derselben Klasse erzeugt, sind aber zwei **verschiedene Instanzen**. Genau wie zwei echte Aufzüge im selben Gebäude zwar nach demselben Bauplan gebaut sein können, aber nicht deshalb derselbe Aufzug sind.

### `__init__()`, `self` und Instanzattribute

Ein echter Aufzug braucht natürlich mehr Informationen. Zum Beispiel:

- Welche Etagen darf er anfahren?
- Auf welcher Etage befindet er sich gerade?
- Ist die Tür offen oder geschlossen?

Diese Informationen sollen jedes `Elevator`-Objekt von Anfang an besitzen. Dafür verwenden wir `__init__()`:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False
```

`__init__()` wird direkt nach dem Erzeugen einer neuen Instanz zur Initialisierung aufgerufen (im Einstieg oft vereinfachend als Konstruktor bezeichnet).

Hier sind `accessible_levels` und `start_level` **Parameter** der Methode. Mit `self.current_level = start_level` wird der übergebene Wert anschließend als **Attribut des erzeugten Objekts** gespeichert.

`self` steht dabei für das konkrete Objekt, das gerade erzeugt oder benutzt wird. Deshalb können zwei Aufzüge mit derselben Klasse unterschiedliche Zustände besitzen:

<!-- pytest-codeblocks:cont -->

```python
elevator_a = Elevator([-1, 0, 1, 2, 3])
elevator_b = Elevator([0, 1, 2, 3], start_level=2)

print(elevator_a.accessible_levels)  # => [-1, 0, 1, 2, 3]
print(elevator_a.current_level)      # => 0
print(elevator_b.current_level)      # => 2
print(elevator_a.door_open)          # => False
```

### Lokale Variable oder Attribut?

Der Unterschied zwischen einer lokalen Variable und einem Attribut ist wichtig:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        levels = list(accessible_levels)  # lokale Variable
        self.accessible_levels = levels   # Attribut
        self.current_level = start_level  # Attribut
        self.door_open = False            # Attribut
```

`levels` existiert nur während des Aufrufs von `__init__()`. `self.accessible_levels` bleibt dagegen am Objekt gespeichert und kann später wieder gelesen oder verändert werden.

Und jedes Objekt besitzt seinen **eigenen Zustand**:

<!-- pytest-codeblocks:cont -->

```python
elevator_a = Elevator([-1, 0, 1, 2, 3])
elevator_b = Elevator([-1, 0, 1, 2, 3])

elevator_a.current_level = 2

print(elevator_a.current_level)  # => 2
print(elevator_b.current_level)  # => 0
```

Die Veränderung von `elevator_a` verändert `elevator_b` nicht.

OK. Kurzes mini-Quiz:

> ### Quiz!
>
> In Python ist `"my name is"` ... (alle zutreffenden Antworten auswählen)
>
> A) ... eine Instanz.
> B) ... eine Klasse.
> C) ... ein Objekt.
> D) ... eine Funktion.

und

> Nächste Frage: `"my name is"` ist ...
>
> A) ein Objekt der Klasse `str` (String)
> B) ein Objekt ohne Klasse
> C) ein Objekt der Klasse `type`

Weiter geht's ...

## Instanzmethoden

Bisher haben unsere Aufzüge zwar einen Zustand, können aber noch nichts tun. Methoden geben den Objekten Verhalten.

Wir beginnen mit einer Methode **ohne zusätzlichen Parameter**. Sie soll den aktuellen Zustand beschreiben:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def description(self):
        door_text = "offen" if self.door_open else "geschlossen"
        return f"Etage {self.current_level}, Tür {door_text}"
```

`door_text` ist wieder nur eine **lokale Variable** der Methode. Die Methode liest Attribute des Objekts, verändert sie aber nicht.

<!-- pytest-codeblocks:cont -->

```python
elevator = Elevator([-1, 0, 1, 2, 3], start_level=1)

before_level = elevator.current_level
before_door = elevator.door_open

text = elevator.description()
print(text)  # => Etage 1, Tür geschlossen

print(elevator.current_level == before_level)  # => True
print(elevator.door_open == before_door)        # => True
```

Die Beschreibung wird als **String zurückgegeben**. Das ist etwas anderes als direkt in der Methode zu `print()`en: Das Hauptprogramm kann nun selbst entscheiden, was mit dem String passieren soll.

### Methoden mit zusätzlichen Parametern

Methoden können genau wie Funktionen weitere Parameter erhalten. Zum Beispiel können wir fragen, wie weit eine gewünschte Etage von der aktuellen Position entfernt ist:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def description(self):
        door_text = "offen" if self.door_open else "geschlossen"
        return f"Etage {self.current_level}, Tür {door_text}"

    def distance_to(self, level):
        return abs(level - self.current_level)
```

`distance_to()` benötigt neben dem Objekt (`self`) noch die gewünschte Etage `level`.

<!-- pytest-codeblocks:cont -->

```python
elevator = Elevator([-1, 0, 1, 2, 3], start_level=1)

distance = elevator.distance_to(3)
print(distance)  # => 2
print(elevator.current_level)  # => 1
```

Auch `distance_to()` ist eine **lesende/berechnende Methode**: Sie berechnet einen Wert aus dem Zustand des Objekts, verändert den Zustand aber nicht.

## Zustandsändernde Methoden

Andere Methoden sollen den Zustand eines Objekts ganz bewusst verändern. Bei unserem Aufzug sind das zum Beispiel `open_door()`, `close_door()` und später `move_to()`.

Dabei ist es hilfreich, wenn die Methode zusätzlich zurückgibt, ob die gewünschte Aktion tatsächlich durchgeführt wurde. Hier verwenden wir dafür `True` oder `False`:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def description(self):
        door_text = "offen" if self.door_open else "geschlossen"
        return f"Etage {self.current_level}, Tür {door_text}"

    def distance_to(self, level):
        return abs(level - self.current_level)

    def open_door(self):
        if self.door_open:
            return False
        self.door_open = True
        return True

    def close_door(self):
        if not self.door_open:
            return False
        self.door_open = False
        return True

    def move_to(self, level):
        if self.door_open:
            return False
        if level not in self.accessible_levels:
            return False
        if level == self.current_level:
            return False

        self.current_level = level
        return True
```

Jetzt können wir verschiedene Zustandsänderungen gezielt ausprobieren:

<!-- pytest-codeblocks:cont -->

```python
elevator = Elevator([-1, 0, 1, 2, 3])

print(elevator.open_door())   # => True: Tür wurde geöffnet
print(elevator.open_door())   # => False: Tür war schon offen
print(elevator.close_door())  # => True: entgegengesetzte Änderung
print(elevator.door_open)     # => False
```

Die **Rückgabe** und der **Folgezustand** sind zwei verschiedene Dinge. `open_door()` gibt zum Beispiel `True` zurück und verändert gleichzeitig `self.door_open`.

Auch beim Fahren können wir jetzt prüfen, ob eine Aktion erlaubt war:

<!-- pytest-codeblocks:cont -->

```python
print(elevator.move_to(2))   # => True
print(elevator.current_level)  # => 2

print(elevator.move_to(2))   # => False: schon auf Etage 2
print(elevator.move_to(8))   # => False: Etage nicht erreichbar
print(elevator.current_level)  # => 2

elevator.open_door()
print(elevator.move_to(3))   # => False: Tür ist offen
print(elevator.current_level)  # => 2
```

Das Beispiel zeigt einen wichtigen Unterschied:

- `description()` und `distance_to()` **lesen/berechnen** etwas.
- `open_door()`, `close_door()` und `move_to()` **verändern den Zustand** des Objekts.

## Statische Methoden (`@staticmethod`)

Nicht jede Funktionalität, die thematisch zu einer Klasse gehört, benötigt tatsächlich ein konkretes Objekt.

Für die Anzeige eines Stockwerks wollen wir zum Beispiel folgende Bezeichnungen verwenden:

- `-1` → `UG`
- `0` → `EG`
- `1` → `1. OG`
- `2` → `2. OG`

Dafür benötigen wir weder `current_level` noch irgendein anderes Attribut eines bestimmten Aufzugs. Solch eine Methode kann als **statische Methode** definiert werden:

```python
class Elevator:
    @staticmethod
    def level_label(level):
        if level == -1:
            return "UG"
        if level == 0:
            return "EG"
        return f"{level}. OG"
```

Eine statische Methode hat kein `self`. Wir können sie direkt über die Klasse aufrufen:

<!-- pytest-codeblocks:cont -->

```python
print(Elevator.level_label(-1))  # => UG
print(Elevator.level_label(0))   # => EG
print(Elevator.level_label(3))   # => 3. OG
```

Die Entscheidung lautet also grob:

- Braucht die Methode **Daten eines konkreten Objekts**? → Instanzmethode mit `self`.
- Gehört die Funktion logisch zur Klasse, braucht aber **keinen Objektzustand**? → eventuell `@staticmethod`.

## Eine Klasse, viele Objekte

Es können beliebig viele Objekte einer Klasse erzeugt werden. Die Klasse bleibt dabei dieselbe, die Zustände der einzelnen Objekte können aber unterschiedlich sein.

Für das folgende Beispiel verwenden wir noch einmal unsere vollständige Aufzugsklasse:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def description(self):
        door_text = "offen" if self.door_open else "geschlossen"
        level_text = self.level_label(self.current_level)
        return f"{level_text}, Tür {door_text}"

    def distance_to(self, level):
        return abs(level - self.current_level)

    def open_door(self):
        if self.door_open:
            return False
        self.door_open = True
        return True

    def close_door(self):
        if not self.door_open:
            return False
        self.door_open = False
        return True

    def move_to(self, level):
        if self.door_open:
            return False
        if level not in self.accessible_levels:
            return False
        if level == self.current_level:
            return False
        self.current_level = level
        return True

    @staticmethod
    def level_label(level):
        if level == -1:
            return "UG"
        if level == 0:
            return "EG"
        return f"{level}. OG"
```

Nun können wir mehrere Aufzüge in einer Liste verwalten:

<!-- pytest-codeblocks:cont -->

```python
elevators = [
    Elevator([-1, 0, 1, 2, 3], start_level=0),
    Elevator([0, 1, 2, 3, 4], start_level=2),
    Elevator([-1, 0, 1], start_level=-1),
]

for elevator in elevators:
    print(elevator.current_level)
    print(elevator.description())
```

Hier enthält die Liste nicht Zahlen oder Strings, sondern drei selbst erzeugte `Elevator`-Objekte. In der Schleife können wir ihre Attribute lesen und ihre Methoden aufrufen.

## Ein zweiter kleiner Datentyp: `Display`

Ein größerer Vorteil von OOP wird sichtbar, wenn verschiedene Objekte unterschiedliche Aufgaben übernehmen.

Ein Aufzug könnte zum Beispiel ein kleines Display besitzen. Das Display muss nicht wissen, wie ein Aufzug fährt oder welche Türen geöffnet werden dürfen. Seine Aufgabe ist nur, eine Nachricht anzuzeigen bzw. zu speichern:

```python
class Display:
    def __init__(self):
        self.message = ""

    def show(self, message):
        self.message = message
        return self.message
```

<!-- pytest-codeblocks:cont -->

```python
display = Display()
print(display.show("↑ 2. OG"))  # => ↑ 2. OG
print(display.message)           # => ↑ 2. OG
```

### Objekte die andere Objekte enthalten ("Komposition")

Eine Klasse kann auch Objekte einer anderen Klasse als Attribut enthalten. Das nennt man **Komposition**.

Wir können beispielsweise jedem Aufzug ein `Display` geben. Die folgende verkürzte Version konzentriert sich nur auf diesen neuen Aspekt:

```python
class Display:
    def __init__(self):
        self.message = ""

    def show(self, message):
        self.message = message
        return self.message


class Elevator:
    def __init__(self, accessible_levels, display, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False
        self.display = display

    def move_to(self, level):
        if self.door_open:
            return False
        if level not in self.accessible_levels:
            return False
        if level == self.current_level:
            return False

        direction = "↑" if level > self.current_level else "↓"
        self.display.show(f"{direction} {self.level_label(level)}")
        self.current_level = level
        return True

    @staticmethod
    def level_label(level):
        if level == -1:
            return "UG"
        if level == 0:
            return "EG"
        return f"{level}. OG"
```

<!-- pytest-codeblocks:cont -->

```python
main_display = Display()
elevator = Elevator([-1, 0, 1, 2, 3], main_display)

elevator.move_to(2)
print(elevator.current_level)   # => 2
print(main_display.message)     # => ↑ 2. OG
```

Hier macht der Aufzug nicht selbst die komplette Anzeige. Stattdessen besitzt er ein `Display`-Objekt und ruft dessen Methode `show()` auf. Genau solche Zusammenarbeit zwischen Objekten wird uns bei größeren OOP-Beispielen wieder begegnen.

## Klassen in eigenen Dateien

Genau wie Funktionen können Klassen in eigenen Python-Dateien liegen. In einem echten Projekt würden wir die Klassendefinition deshalb häufig vom eigentlichen Hauptprogramm trennen.

Zum Beispiel könnte `elevator.py` unsere Klasse enthalten:

<!-- pytest-codeblocks:skip -->

```python
# elevator.py
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def move_to(self, level):
        if level not in self.accessible_levels:
            return False
        self.current_level = level
        return True
```

Und das Hauptprogramm liegt in einer zweiten Datei `main.py`:

<!-- pytest-codeblocks:skip -->

```python
# main.py
import elevator

building_elevator = elevator.Elevator([-1, 0, 1, 2, 3])
building_elevator.move_to(2)
print(building_elevator.current_level)
```

Hier verwenden wir bewusst `import elevator` und anschließend `elevator.Elevator(...)`. Damit ist direkt sichtbar, **aus welchem Modul** die Klasse kommt.

Die beiden Codeblöcke sind für `pytest-codeblocks` mit `skip` markiert, weil sie zwei verschiedene Dateien darstellen und daher nicht als einzelner Markdown-Codeblock ausgeführt werden können.

## Eine kleine Testspur: Aufruf → Rückgabe → Folgezustand

Gerade bei zustandsändernden Methoden hilft eine kleine Testspur. Wir schauen dabei nicht nur auf die Rückgabe, sondern auch darauf, was danach im Objekt gespeichert ist.

Dafür verwenden wir noch einmal eine kompakte Version unserer Klasse:

```python
class Elevator:
    def __init__(self, accessible_levels, start_level=0):
        self.accessible_levels = list(accessible_levels)
        self.current_level = start_level
        self.door_open = False

    def open_door(self):
        if self.door_open:
            return False
        self.door_open = True
        return True
```

<!-- pytest-codeblocks:cont -->

```python
elevator_a = Elevator([-1, 0, 1, 2, 3])
elevator_b = Elevator([-1, 0, 1, 2, 3])

result = elevator_a.open_door()

print("Aufruf: elevator_a.open_door()")
print("Rückgabe:", result)                  # => True
print("Folgezustand A:", elevator_a.door_open)  # => True
print("Folgezustand B:", elevator_b.door_open)  # => False

assert result is True
assert elevator_a.door_open is True
assert elevator_b.door_open is False
```

Damit prüfen wir drei Dinge getrennt:

1. **Was wurde aufgerufen?**
2. **Was wurde zurückgegeben?**
3. **Wie sieht der Objektzustand danach aus?**

Und gleichzeitig sehen wir noch einmal: `elevator_b` bleibt vollständig unabhängig von der Änderung an `elevator_a`.

Super.

Wir können jetzt eigene Klassen definieren, daraus Objekte erzeugen und Zustände und Verhalten sinnvoll zusammenfassen.

So what!? Was bringt das denn jetzt überhaupt?

Eine ganze Reihe! Klassen helfen dabei Programme sinnvoll zu strukturieren. Mit Klassen können wir Objekte so entwerfen, dass sie genau die gewünschten Eigenschaften besitzen und die dazu passenden Methoden mitbringen. Bei unserem Aufzug liegen z.B. Position und Türstatus nicht als lose Variablen irgendwo im Programm, sondern gehören zum jeweiligen `Elevator`-Objekt. Gleichzeitig können mehrere Aufzüge mit derselben Klasse erzeugt werden, ohne dass ihre Zustände miteinander vermischt werden.
