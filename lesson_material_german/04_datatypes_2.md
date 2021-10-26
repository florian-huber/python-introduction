# Datentypen II

Wir haben schon einige Datentypen gesehen. Jetzt wollen wir aber noch einmal genauer darauf eingehen und weitere wichtige Typen besprechen.

### Variablen und Speicher

Wichtig zu verstehen ist, dass Variablen in Python nicht die Daten/Werte enthalten. Es sind nur Namen mit der Information wo im Speicher der Wert steht.

Daher gibt es sowas:

```python 
a = [5]
b = a
b.append(4) 
print(a)
```

---

Kurze Wiederholung der Datentypen mit denen wir schon gearbeitet haben

### Zahlen

Hier hatten wir schon häufiger die Integer und Floats (floating point number).

```python
type(6)  # => int
type(6.0)  # => float
```

Hatten wir noch nicht hier, und benötigen wir im Moment auch noch nicht.
Aber nur der Vollständigkeit halber: Komplexe Zahlen.

```python
type(2j +3)  # => complex
```

 

### Strings

```python
type("abcdefg")
type('abcdefg')
type("""abcdefg""")  # z.B. bei den "Docstrings" von Funktionen
```

### Lists & Tuples

Wir hatten schon Lists und Tuple, die auf den ersten Blick ziemlich ähnlich sind.

```python
my_list = ["abc", "ABC", 5, 0.315]
my_tuple = ("abc", "ABC", 5, 0.315)

print(my_list[-1])
print(my_tuple[-1])
```

In der Handhabung unterscheiden sich beide Datentypen aber sehr stark voneinander! 

Der Kernunterschied ist: `list` ist veränderbar und kommt mit vielen Methoden/Funktionen!

```python
my_list = ["abc"]

my_list.append("noch was")
my_list.insert(1, "xyz")
my_list[2] = "XYZ"
print(my_list)  # => ['abc', 'xyz', 'XYZ']
```

---

Es gibt in Python aber noch 2 weitere sehr wichtige Datentypen:

## Sets und Dictionaries

### Sets (Mengen)

```python
my_set = {"yes", "no", "maybe"}
```

Sieht erstmal wieder fast aus wie bei den Tuples und Lists. Allerdings macht das folgende Beispiel gleich klar,  dass Sets ein ganz anderes Format darstellen:

<!-- pytest-codeblocks:expect-error -->

```python
my_set[0]  # => TypeError: 'set' object is not subscriptable
```

Sets sind keine Sequenzen! Elemente haben keine Indizes. Andere Dinge die bei List und/oder Tuple funktionieren gehen aber:

```python
my_set = {"yes", "no", "maybe"}

"maybe" in my_set  # => True
```

Sogar ein `for`-Loop geht:

<!-- pytest-codeblocks:cont -->

```python
for s in my_set:
    print(f"Well, {s}...")
```

Die Unterschiede zwischen Sets (deutsch: Mengen) und Listen passen weitestgehend zur Verwendung der Begriffe im Alltag oder in der Mathematik.

Wenn ich frage würde: Schreiben Sie eine Liste mit allen Tieren die Ihnen einfallen, dann kann es sein, dass auf der Liste einige Tiere mehrfach erscheinen...

Wenn ich aber frage: Was ist die Menge aller Ihnen bekannten Tiere, dann würden Sie aber sicherlich keine Tiere doppelt zählen!

```python
animals = {"cat", "dog", "elephant", "lion", "dog"}
print(animals)
```

Noch deutlicher wird das Ganze wenn wir Mengen zusammenführen.

Stellen Sie sich vor, mehrere von ihnen schreiben eine Liste mit Tieren auf (`animal_list`).

Jetzt wollen wir diese zusammenführen...

```python
animal_list1 = ["cat", "dog", "elephant", "lion"]
animal_list2 = ["parrot", "cat", "dog", "goldfish", "cow"]

all_animals = animal_list1 + animal_list2
print(all_animals)  # => Oh no, 2x dog and cat :(
```

Bei Sets geht das Gleiche aber nicht:

<!-- pytest-codeblocks:expect-error -->

```python
animal_set1 = {"cat", "dog", "elephant", "lion"}
animal_set2 = {"parrot", "cat", "dog", "goldfish", "cow"}

all_animals = animal_set1 + animal_set2
print(all_animals)  # => TypeError: unsupported operand type(s) for +: 'set' and 'set'
```

Ok, bei Sets geht das also anders:

<!-- pytest-codeblocks:cont -->

```python
all_animals = animal_set1.union(animal_set2)
print(all_animals)  # => Yes, that works!
```

Wo wir gerade bei Mengen sind: Damit kann man natürlich noch viel mehr machen, z.B:

```python
animal_set1 = {"cat", "dog", "elephant", "lion"}
animal_set2 = {"parrot", "cat", "dog", "goldfish", "cow"}

most_popular = animal_set1.intersection(animal_set2)
print(f"Named twice: {most_popular}")
```

> ### Mini-QUIZ! Welche Tiere werden hiermit ausgegeben?
>
> a) {'parrot', 'goldfish', 'cow'}
> b) {'cat', 'dog'}
> c) {'elephant', 'lion'}
> d) {}

Sets können auch erweitert werden mit `add()` oder `update()`. Elemente können auch entfernt werden mit `remove()`.

```python
animals = {"cat", "dog", "elephant", "lion"}
animals.add("cow")
animals.update({"wolf", "goat"})
animals.remove("lion")
print(animals)
```

Es können auch einfach Sequenzen (list, str) in ein Set umgewandelt werden

```python
s = "dann am abend baden mit diamentendieben die bananen dabei haben"
```

> ### Wie viele verschiedene Buchstaben hat der Satz?

```python
characters = set(s)
print(characters)
print(f"Anzahl der Zeichen im string: {len(s)}")
print(f"Aber nur {len(characters)} einzigartige Zeichen!")
```

## Dictionaries

Dictionaries sind ein häufig verwendeter Datentyp mit Daten als Schlüssel-Werte Paare genutzt werden können. D.h. die einzelnen Werte (oder Objekte) - `value` sind immer einem Schlüssel `key`zugeordnet.

```python
my_dict = {key1: value1,
          key2: value2}
```

Ein Beispiel könnte eine Zuordnung von Wörtern in zwei verschiedenen Sprachen sein, hier Deutsch-Englisch:

```python
my_dict = {"Hallo": "hello",
           "Tschüss": "bye",
           "Danke": "thanks"}

print(my_dict["Danke"])
```

Wie bei Sets, gibt es auch in Dictionaries keine Dopplungen:

```python
my_dict = {"Hallo": "hello",
           "Tschüss": "bye",
           "Danke": "thanks",
           "Hallo": "hi"}

print(my_dict["Hallo"])  # => 'hi'
```

Einzelne Elemente aus einem Dictionary können mit dem entsprechenden Schlüssel ausgelesen werden:

```python
my_dict = {"name": "Gilderoy Lockhart",
           "function": "teacher",
           "password": "goldlocks"}

x = my_dict["password"]

# Alternativ geht auch:
x = my_dict.get("password")
```

Genau wie List oder Set ist auch das Dictionary in Python veränderbar (mutable). Das bedeutet, dass Einträge hinzugefügt, entfernt, oder verändert werden können.

<!-- pytest-codeblocks:cont -->

```python
my_dict["password"] = "prettyfl0wer"
print(my_dict["password"])  # => 'prettyfl0wer'
```

Neue key-value Paare können einfach hinzugefügt werden:

<!-- pytest-codeblocks:cont -->

```python
my_dict["hair color"] = "blond"
print(my_dict)
```

Es lässt sich auch abfragen ob ein bestimmter Schlüssel enthalten ist:

<!-- pytest-codeblocks:cont -->

```python
print("password" in my_dict)  # => True
```

Um den gesamten Inhalt eines Dictionary abzufragen gibt es u.a. folgende Optionen:

<!-- pytest-codeblocks:cont -->

```python
print(my_dict.keys())  # alle keys
print(my_dict.values())  # alle Werte (values)
print(my_dict.items())  # alle key-value Paare
```

Mit diesen Optionen, können auch `for`-Loops genutzt werden, z.B. hier mit  `.items()`:

```python
for key, value in my_dict.items():
    print(f"{key} is {value}.")
```





# from Python 3.7 onwards, dictionaries are ordered!



#%%
my_dict = {"name": "Gilderoy Lockhart",
           "function": "teacher",
           "password": "goldlocks"}

x = my_dict["password"]
x = my_dict.get("password")

#%% is changable
my_dict["password"] = "prettyfl0wer"
print(my_dict["password"])

#%% Ask if key in dictionary
print("password" in my_dict)

#%% Access whole dictionary
print(my_dict.keys())
print(my_dict.values())

print(my_dict.items())

#%%
for key, value in my_dict.items():
    print(f"{key} is {value}.")

#%%
for x in my_dict:
    print(x)
    
#%% Add items
my_dict["hair color"] = "blond"
print(my_dict)

#%%

## Nested dictionaries

Bei Listen hatten wir besprochen, dass eine Liste auch weitere Listen enthalten kann. Damit können also verschachtelte Listen erstellt werden. Das Gleiche geht auch für Dictionaries, d.h. ein Dictionary kann weitere Dictionaries enthalten. Damit können auch komplexere key-value Logiken abgebildet werden.

```python
hogwarts = {
    "Dumbeldore": {
        "surname": "Albus",
        "function": "headmaster"
        },
    "Lockhart": {
        "surname": "Gilderoy",
        "function": "teacher"
        },
    "Granger" : {
        "surname": "Hermione",
        "function": "pupil"
        },
} 


print(hogwarts["Granger"]["surname"])  # => Hermione
```

Bei den entsprechenden Einrückungen hilft zum Glück das IDE (Spyder, Visual Studio Code PyCharm etc.), denn das kann zu Beginn schnell etwas unübersichtlich werden. Und Python erlaubt oft verschiedene Einrückungen, z.B. ginge auch sowas:

```python
hogwarts = {"Dumbeldore": {"surname" : "Albus",
                           "function" : "headmaster"},
            "Lockhart": {"surname": "Gilderoy",
                         "function": "teacher"},
            "Granger": {"surname": "Hermione",
                        "function": "pupil"},
            } 


print(hogwarts["Granger"]["surname"])  # => Hermione
```

## Vorsicht: Hier mal etwas das nicht sonderlich intuitiv ist...

Oft möchten wir bestimmte Objekte (zum Beispiel in Dictionary) kopieren. Etwa, wenn wir eine Kopie von einem Dictionary machen wollen um diese danach zu verändern. 

```python
hogwarts_book7 = hogwarts.copy()
hogwarts_book7["Dumbeldore"]["function"] = "former headmaster"

print(hogwarts["Dumbeldore"]["function"])  # => former headmaster
```

Das entspricht jetzt nicht unbedingt dem Ergebnis das erwartet wurde. Wir haben hier das Dictionary `hogwarts_book7` verändert, aber damit (ohne es zu wollen) auch den Eintrag im ursprünglichen Dictionary `hogwarts` angepasst.

Das liegt daran, dass wir hier eine sogenannte flache Kopie (**shallow copy**) erstellt haben.
Hierbei wird das Dictionary nicht komplett kopiert, sondern manchen Element im Speicher wird nur ein neuer Namen zugewiesen. 

Für einfache Dictionaries funktioniert das Ganze:

```python
my_dict = {1: "one", 2: "two"}
new_dict = my_dict.copy()
new_dict[1] = "Eins"
print(my_dict)  # => {1: "one", 2: "two"}
```

Nur für verschachtelte Dictionary klappt das leider nicht mehr.
Hier brauchen wir keine *shallow*, sondern eine *deep* (also tiefe) Kopie:

```python
import copy

hogwarts_book7 = copy.deepcopy(hogwarts)
hogwarts_book7["Dumbeldore"]["function"] = "former headmaster"

print(hogwarts["Dumbeldore"]["function"])  # => headmaster
```

