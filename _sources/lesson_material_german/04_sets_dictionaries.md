# Sets, Dictionaries, and Deep Copy

Wir haben schon einige Datentypen gesehen. Jetzt wollen wir aber noch einmal genauer darauf eingehen und weitere wichtige Typen besprechen. Wir starten mit einer kurzen Wiederholung.

Wichtig zu verstehen ist, dass Variablen in Python Namen sind, die auf Objekte verweisen. Zwei Namen können sich deshalb auf **dieselbe veränderliche Liste** beziehen.

Daher gibt es so etwas:

```python
a = [5]
b = a
b.append(4)
print(a)  # => [5, 4]
```

---

Kurze Wiederholung der Datentypen, mit denen wir schon gearbeitet haben:

## Zahlen

Hier hatten wir schon häufiger Integer und Floats (*floating-point numbers*).

```python
print(type(6))    # => <class 'int'>
print(type(6.0))  # => <class 'float'>
```

Hatten wir noch nicht, und benötigen wir im Moment auch nicht. Aber nur der Vollständigkeit halber: komplexe Zahlen.

```python
print(type(2j + 3))  # => <class 'complex'>
```

## Strings

```python
print(type("abcdefg"))
print(type('abcdefg'))
print(type("""abcdefg"""))  # Dreifache Anführungszeichen werden für Blockkommentare genutzt.
```

## Lists & Tuples

Wir hatten schon Listen und Tupel, die auf den ersten Blick ziemlich ähnlich sind.

```python
my_list = ["abc", "ABC", 5, 0.315]
my_tuple = ("abc", "ABC", 5, 0.315)

print(my_list[-1])
print(my_tuple[-1])
```

In der Handhabung unterscheiden sich beide Datentypen aber deutlich voneinander!

Der Kernunterschied ist: `list` ist veränderbar (*mutable*), `tuple` nicht. Listen besitzen deshalb unter anderem Methoden zum Ergänzen und Verändern von Einträgen:

```python
my_list = ["abc"]

my_list.append("noch was")
my_list.insert(1, "xyz")
my_list[2] = "XYZ"
print(my_list)  # => ['abc', 'xyz', 'XYZ']
```

## Boolean und None

Weitere Typen, die wir jetzt schon häufiger gesehen haben, sind `bool` und `NoneType`. Ein Boolean kann nur `True` oder `False` sein. `None` ist ein besonderer Wert, der häufig „kein Ergebnis“ oder „nicht vorhanden“ bedeutet.

```python
print(type(True))   # => <class 'bool'>
print(type(None))   # => <class 'NoneType'>
```

---

Es gibt in Python noch zwei weitere sehr häufig verwendete Datentypen:

## Sets und Dictionaries

### Sets (Mengen)

```python
my_set = {"yes", "no", "maybe"}

# Oder: aus einer Liste erzeugen
zweites_set = set(["yes", "no", "yes"])

leeres_set = set()
leeres_dict = {}  # Achtung: Das ist ein Dictionary, kein Set!
print(type(leeres_set))
print(type(leeres_dict))
```

Sieht erst einmal wieder fast aus wie bei Tupeln und Listen. Allerdings macht das folgende Beispiel gleich klar, dass Sets ein anderer Datentyp sind:

<!-- pytest-codeblocks:expect-error -->

```python
my_set[0]  # => TypeError: 'set' object is not subscriptable
```

Sets sind keine Sequenzen: Ihre Elemente haben **keine Indizes**. Andere Dinge, die bei Listen oder Tupeln funktionieren, gehen aber auch:

```python
my_set = {"yes", "no", "maybe"}

print("maybe" in my_set)  # => True
print(len(my_set))         # => 3
```

Sogar ein `for`-Loop geht:
<!-- pytest-codeblocks:cont -->

```python
for s in my_set:
    print(f"Well, {s}...")
```

**Wichtig:** Sets garantieren keine feste Durchlauf- oder Ausgabereihenfolge. Die angezeigten Elemente können also in anderer Reihenfolge erscheinen, ohne dass sich der Inhalt der Menge unterscheidet.

Die Unterschiede zwischen Sets (deutsch: Mengen) und Listen passen weitgehend zur Verwendung der Begriffe im Alltag oder in der Mathematik.

Schauen wir uns das Ganze mal mit Tieren an...

```python
animals = {"cat", "dog", "elephant", "lion", "dog"}
print(animals)
print(len(animals))  # => 4: 'dog' ist nur einmal enthalten.
```

Noch deutlicher wird das Ganze, wenn wir Mengen zusammenführen.

Stellen Sie sich vor, mehrere von Ihnen schreiben eine Liste mit Tieren auf (`animal_list`). Jetzt wollen wir diese zusammenführen:

```python
animal_list1 = ["cat", "dog", "elephant", "lion"]
animal_list2 = ["parrot", "cat", "dog", "goldfish", "cow"]

all_animals = animal_list1 + animal_list2
print(all_animals)  # Hier kommen 'cat' und 'dog' doppelt vor.
```

Bei Sets geht das mit `+` nicht:

<!-- pytest-codeblocks:expect-error -->

```python
animal_set1 = {"cat", "dog", "elephant", "lion"}
animal_set2 = {"parrot", "cat", "dog", "goldfish", "cow"}

all_animals = animal_set1 + animal_set2
print(all_animals)  # => TypeError: unsupported operand type(s) for +: 'set' and 'set'
```

Bei Sets gibt es nämlich unterschiedliche Mengenoperationen. Für die **Vereinigung** (*union*) verwenden wir `|` (in der Mathematik: \(A \cup B\)). Sie enthält alle Elemente aus beiden Mengen, aber jedes nur einmal:
<!-- pytest-codeblocks:cont -->

```python
all_animals = animal_set1 | animal_set2
print(all_animals)

# Dieselbe Operation als Methode:
print(animal_set1.union(animal_set2))
```

Es gibt aber noch ganz andere Verknüpfungen, zum Beispiel die **Schnittmenge** (*intersection*, \(A \cap B\)). Sie enthält nur Elemente, die in beiden Mengen vorkommen:

<!-- pytest-codeblocks:cont -->

```python
most_popular = animal_set1 & animal_set2
print(f"In beiden Mengen: {most_popular}")

# Entspricht auch: animal_set1.intersection(animal_set2)
```

> ### Mini-QUIZ! Welche Tiere sind in der Schnittmenge enthalten?
>
> a) `{'parrot', 'goldfish', 'cow'}`  
> b) `{'cat', 'dog'}`  
> c) `{'elephant', 'lion'}`  
> d) `set()`
>
> Die Reihenfolge innerhalb eines Sets ist dabei egal.

Die **Differenz** `a - b` enthält dagegen nur Elemente aus `a`, die **nicht** in `b` vorkommen. Die Richtung macht einen Unterschied. Auch die *symmetrische Differenz* `a ^ b` ist möglich: Elemente, die in genau einer der Mengen vorkommen.

Ein kleines Beispiel mit überschaubaren Zahlen zeigt alle vier wichtigen Operatoren:

```python
a = {1, 2, 3}
b = {3, 4}

print(a | b)  # => {1, 2, 3, 4}  Vereinigung
print(a & b)  # => {3}           Schnittmenge
print(a - b)  # => {1, 2}        Nur in a
print(b - a)  # => {4}           Nur in b
print(a ^ b)  # => {1, 2, 4}     In genau einer Menge
```

Wie verhalten sich diese Operationen, wenn eine Menge leer ist oder die Mengen keine gemeinsamen Elemente haben?

```python
a = {1, 2}
leer = set()

print(a | leer)      # => {1, 2}
print(a & leer)      # => set()
print(a - leer)      # => {1, 2}
print(leer - a)      # => set()
print(a & {3, 4})    # => set()
```

Sets können mit `.add()` um einzelne Elemente oder mit `.update()` um mehrere Elemente erweitert werden. Mit `.remove()` können Elemente entfernt werden (fehlt das Element, entsteht dabei ein `KeyError`).

```python
animals = {"cat", "dog", "elephant", "lion"}
animals.add("cow")
animals.add("cow")  # Keine Dopplung
animals.update({"wolf", "goat"})
animals.remove("lion")
print(animals)
print(len(animals))
```

Sequenzen (zum Beispiel Listen und Strings) lassen sich in Sets umwandeln. Ein Set aus einem String enthält seine **unterschiedlichen Zeichen**, also zunächst auch Leerzeichen!

```python
s = "dann am abend baden mit diamentendieben die bananen dabei haben"
```

> ### Wie viele verschiedene Buchstaben hat der Satz (ohne Leerzeichen)?

<!-- pytest-codeblocks:cont -->

```python
characters = set(s.replace(" ", ""))
print(characters)
print(f"Anzahl der Zeichen im String (mit Leerzeichen): {len(s)}")
print(f"Aber nur {len(characters)} unterschiedliche Buchstaben!")
```

## Dictionaries

Dictionaries sind ein häufig verwendeter Datentyp, mit dem Daten als **Schlüssel-Wert-Paare** genutzt werden. Jeder Wert (*value*) ist einem Schlüssel (*key*) zugeordnet.

Die allgemeine Schreibweise lautet beispielsweise `{schluessel: wert}`. Ein konkretes Beispiel könnte eine Zuordnung von Wörtern in zwei Sprachen sein, hier Deutsch–Englisch:

```python
my_dict = {
    "Hallo": "hello",
    "Tschüss": "bye",
    "Danke": "thanks"
}

print(my_dict["Danke"])  # => thanks
```

Wie bei Sets gibt es auch hier keine doppelten **Schlüssel**. Allerdings dürfen unterschiedliche Schlüssel durchaus denselben Wert haben. Wenn wir einem bereits vorhandenen Schlüssel einen neuen Wert zuweisen, wird der bisherige Wert ersetzt:

```python
my_dict["Hallo"] = "hi"
print(my_dict["Hallo"])  # => hi
print(len(my_dict))       # => 3, weiterhin drei Schlüssel
```

Anders als Sets bewahren Dictionaries in aktuellem Python die **Einfügereihenfolge ihrer Schlüssel**. Sie sind deshalb trotzdem keine Listen: Der Zugriff erfolgt über Schlüssel und nicht über fortlaufende Positionen.

Einzelne Werte können mit dem entsprechenden Schlüssel ausgelesen werden:

```python
my_dict = {
    "name": "Gilderoy Lockhart",
    "function": "teacher",
    "subject": "Defence Against the Dark Arts"
}

x = my_dict["subject"]
print(x)

# Alternativ geht auch:
x = my_dict.get("subject")
print(x)
```

**Vorsicht bei unbekannten Schlüsseln:** `my_dict["room"]` löst einen `KeyError` aus, wenn der Schlüssel fehlt. Darum prüfen wir vor einem unsicheren Zugriff, ob der Schlüssel existiert. Mit `.get()` lässt sich alternativ ein Ersatzwert angeben.

<!-- pytest-codeblocks:cont -->

```python
if "room" in my_dict:
    print(my_dict["room"])
else:
    print("Kein Raum eingetragen")

print(my_dict.get("room", "unbekannt"))  # => unbekannt
```

Genau wie Listen und Sets sind auch Dictionaries veränderbar (*mutable*). Das bedeutet, dass Einträge hinzugefügt, entfernt oder verändert werden können:

<!-- pytest-codeblocks:cont -->

```python
my_dict["function"] = "former teacher"  # Vorhandenen Wert aktualisieren
print(my_dict["function"])

my_dict["hair color"] = "blond"  # Neuen Eintrag hinzufügen
print(my_dict)
```

Um den gesamten Inhalt eines Dictionarys zu betrachten, gibt es folgende Möglichkeiten:

<!-- pytest-codeblocks:cont -->

```python
print(my_dict.keys())    # alle Schlüssel
print(my_dict.values())  # alle Werte
print(my_dict.items())   # alle Schlüssel-Wert-Paare
```

Mit diesen Möglichkeiten können wir auch `for`-Schleifen nutzen. Ein direkter Durchlauf liefert **Schlüssel**:

<!-- pytest-codeblocks:cont -->

```python
for key in my_dict:
    print(key, "=>", my_dict[key])
```

Mit `.items()` bekommen wir Schlüssel und Wert gleichzeitig. Jedes Paar wird auf zwei Namen verteilt:

<!-- pytest-codeblocks:cont -->

```python
for key, value in my_dict.items():
    print(f"{key} is {value}.")
```

### Häufigkeiten mit einem Dictionary zählen

Listen können Werte mehrfach enthalten, Sets dagegen nicht. Ein Dictionary eignet sich, wenn wir **zusätzlich zählen wollen, wie oft etwas vorkommt**. Wir bauen die Häufigkeiten mit einer Schleife auf:

```python
def zaehle_woerter(woerter):
    haeufigkeiten = {}
    for wort in woerter:
        if wort in haeufigkeiten:
            haeufigkeiten[wort] += 1
        else:
            haeufigkeiten[wort] = 1
    return haeufigkeiten

woerter = ["tee", "kaffee", "tee", "wasser", "tee"]
ergebnis = zaehle_woerter(woerter)
print(ergebnis)       # => {'tee': 3, 'kaffee': 1, 'wasser': 1}
print(type(ergebnis)) # => <class 'dict'>
print(ergebnis["tee"])
print(zaehle_woerter([]))  # => {}
```

Die Funktion erhält eine **Liste** und liefert ein **Dictionary** zurück. Der Eintrag für ein Wort wird beim ersten Auftreten angelegt und bei weiteren Treffern erhöht. Später können wir dafür auch Text mit `.split()` in eine Wortliste verwandeln (siehe *String handling*).

### Liste, Set oder Dictionary?

| Datentyp | Kennzeichen                                                  | Typischer Einsatz                           |
| -------- | ------------------------------------------------------------ | ------------------------------------------- |
| `list`   | Reihenfolge, Indizes, doppelte Einträge möglich              | Werte nacheinander speichern oder verändern |
| `set`    | Eindeutige Elemente, keine Indizes, keine garantierte Reihenfolge | Zugehörigkeit und Mengenoperationen         |
| `dict`   | Eindeutige Schlüssel mit zugeordneten Werten, Einfügereihenfolge bleibt erhalten | Übersetzungen, Eigenschaften, Häufigkeiten  |

## Nested dictionaries

Bei Listen hatten wir besprochen, dass eine Liste weitere Listen enthalten kann. Genauso kann ein Dictionary weitere Dictionaries enthalten. Damit lassen sich komplexere Schlüssel-Wert-Zuordnungen abbilden.

```python
hogwarts = {
    "Dumbledore": {
        "firstname": "Albus",
        "function": "headmaster"
    },
    "Lockhart": {
        "firstname": "Gilderoy",
        "function": "teacher"
    },
    "Granger": {
        "firstname": "Hermione",
        "function": "pupil"
    }
}

print(hogwarts["Granger"]["firstname"])  # => Hermione
```

Bei den entsprechenden Einrückungen hilft zum Glück der Editor bzw. die IDE (z. B. Visual Studio Code oder PyCharm), denn das kann zu Beginn schnell etwas unübersichtlich werden. Der erste Schlüssel wählt hier ein **inneres Dictionary**, der zweite darin einen Wert. Python erlaubt unterschiedliche sinnvolle Formatierungen, solange die Klammern und die Python-Syntax stimmen.

## =, copy(), deepcopy()

**Vorsicht: Hier mal etwas, das nicht sonderlich intuitiv ist ...**

Oft möchten wir bestimmte Objekte (zum Beispiel Dictionaries) kopieren, etwa um eine Kopie anschließend zu verändern.

Dictionaries sind – wie Listen – veränderbare Datentypen in Python. Wir haben schon einmal gesehen, dass dies zu unerwarteten Resultaten führen kann:

```python
my_list = [1, 2, 3]
my_list2 = my_list
my_list2[0] = 4321
print(my_list[0])  # => 4321
```

Das Gleiche passiert bei Dictionaries: Die Zuweisung erzeugt **kein neues Dictionary-Objekt**, sondern einen zweiten Namen für dasselbe Objekt. Eine mögliche Lösung stellt die Methode `.copy()` dar.

```python
my_list = [1, 2, 3]
my_list2 = my_list.copy()
my_list2[0] = 4321
print(my_list[0])  # => 1
```

Das geht auch bei Dictionaries:

```python
my_dict = {
    "name": "Gilderoy Lockhart",
    "function": "teacher",
    "subject": "Defence Against the Dark Arts"
}
my_dict2 = my_dict.copy()
my_dict2["function"] = "former teacher"
print(my_dict["function"])  # => teacher
```

Sobald wir aber mit tiefer verschachtelten **veränderlichen** Objekten arbeiten, ergeben sich erneut Probleme:

<!-- pytest-codeblocks:cont -->

```python
hogwarts = {
    "Dumbledore": {
        "firstname": "Albus",
        "function": "headmaster"
    },
    "Lockhart": {
        "firstname": "Gilderoy",
        "function": "teacher"
    },
    "Granger": {
        "firstname": "Hermione",
        "function": "pupil"
    }
}

hogwarts_book7 = hogwarts.copy()
hogwarts_book7["Dumbledore"]["function"] = "former headmaster"

print(hogwarts["Dumbledore"]["function"])  # => former headmaster
print(hogwarts is hogwarts_book7)              # => False
print(hogwarts["Granger"] is hogwarts_book7["Granger"])  # => True
```

Das entspricht jetzt nicht unbedingt dem erwarteten Ergebnis. Wir haben `hogwarts_book7` verändert, aber damit (ohne es zu wollen) auch einen Eintrag im ursprünglichen Dictionary angepasst.

Das liegt daran, dass wir mit `.copy()` eine sogenannte **flache Kopie** (*shallow copy*) erstellt haben. Die äußeren Dictionaries sind getrennte Objekte, ihre **inneren Dictionaries** werden hier weiterhin gemeinsam verwendet.

Für verschachtelte Dictionaries klappt das also nicht immer so einfach. Mit `copy.deepcopy()` können wir auch die enthaltenen veränderlichen Objekte rekursiv kopieren:

```python
import copy

hogwarts = {
    "Dumbledore": {
        "firstname": "Albus",
        "function": "headmaster"
    },
    "Lockhart": {
        "firstname": "Gilderoy",
        "function": "teacher"
    },
    "Granger": {
        "firstname": "Hermione",
        "function": "pupil"
    }
}

hogwarts_book7 = copy.deepcopy(hogwarts)
hogwarts_book7["Dumbledore"]["function"] = "former headmaster"

print(hogwarts["Dumbledore"]["function"])  # => headmaster
```

Das Gleiche gilt übrigens auch für Listen:

```python
import copy

my_list1 = [[1, 2, 3], 12, 15]
my_list2 = copy.deepcopy(my_list1)

my_list2[0][0] = 54321
print(my_list1[0])  # => [1, 2, 3]
```

**Anmerkung:** Eine tiefe Kopie erstellt bei Bedarf neue Kopien der enthaltenen Objekte. Unveränderliche Objekte wie Zahlen und Strings müssen dabei nicht zwingend neu angelegt werden. Für einfache Listen oder Dictionaries reicht oft `.copy()`, aber bei verschachtelten veränderlichen Objekten ist der Unterschied wichtig.