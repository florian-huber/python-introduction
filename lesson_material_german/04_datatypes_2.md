# # Datentypen II

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

