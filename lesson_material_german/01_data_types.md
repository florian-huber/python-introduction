# Datentypen (*data types*)

Datentypen in Python verbinden Werte (oder Daten) mit Methoden, die festlegen, welche Operationen auf diesen Daten durchgeführt werden können. Die Wahl des Datentyps bestimmt also, welche Aktionen möglich sind und wie diese umgesetzt werden.

In Python gibt es viele verschiedene Datentypen. Für den Einstieg beginnen wir mit zwei grundlegenden Typen: **Zahlen** und **Strings**.

## Zahlen (*numbers*)

Das Arbeiten mit Zahlen in Python ist sehr intuitiv und verhält sich ähnlich wie bei einem Taschenrechner.
Bei den Zahlen unterscheidet Python zwischen **Integer** (`int`) und **Float** (von engl.: "floating point number", also Gleitkommazahlen). **Integer** sind ganze Zahlen (z. B. `5`). **Float** sind Kommazahlen (z. B. `5.01`).

Anmerkung: Floats entsprechen nicht exakt den mathematischen reellen Zahlen, da Computer bei Gleitkommazahlen eine Rundung vornehmen.

### Beispiele:

```py
>>> 5
5

>>> 5.01
5.01
```

Vorzeichen gehen so:
```python
# Positiv
+5  # 5

# Negativ
-5  # -5
```

Und, wie gesagt, grundlegende Rechenoperationen gehen wie zu erwarten:
```python
# Addition
5 + 7  # 12

# Subtraktion
5 - 7  # -2

# Multiplikation
5 * 7  # 35

# Division
5 / 2  # 2.5
```

**Hinweis**: Die Division in Python gibt immer einen `float`-Wert zurück, auch wenn das Ergebnis eine ganze Zahl wäre:

```python
print(type(5 / 1))
```
ergibt:
```
<class 'float'>
```
Dieses Beispiel zeigt auch, dass wir den Datentypen in Python erfragen können mit`type()`.

### Erweiterte Rechenoperationen:
```python
# Ganzzahlige Division
5 // 2  # 2

# Modulo (Rest einer Division)
5 % 2  # 1

# Potenzierung
10 ** 2  # 100
```

## Print und Rechenoperationen verbinden

Mehrere Rechenoperationen können hintereinander ausgeführt und mit der `print()`-Funktion ausgegeben werden:
```python
print(22 * 27 * 7 / 2)
```
ergibt:

```
2079.0
```

Auch Klammern können eingesetzt werden:
```python
print((22 * 27 + 12) / 2)
print(22 * 27 + 12 / 2)
```
was das Folgende ausgibt:
```
303.0
600.0
```
Klammern können außerdem verwendet werden, um die Lesbarkeit des Codes zu erhöhen, auch wenn sie mathematisch nicht notwendig sind:

```python
print((22 * 27 * 7) / 2)  # völlig in Ordnung!
```


Andere häufige Operationen mit Zahlen:
```python
# // integer division (ganzzahlige Division)
5 // 2  # 2 (this is NOT the same as rounding!)

# % modulo
5 % 2  # 1

# ** power (Potenz)
10 ** 2  # 100
```

## Strings

Strings sind Zeichenketten (oder Sequenzen von Zeichen) und werden mit  `'` oder `"` definiert.
```
>>> "5 + 7"
'5 + 7'
```
```
>>> "Whatever you want to type. 123*..."
'Whatever you want to type. 123*...'
```

### Quiz: Welche der folgenden Anweisungen gibt keinen Fehler zurück?
```
>>> "five" + "five"

>>> "five" + 5

>>> 2 * "five"

>>> "five" - "ive"
```
(Einfach einmal selber ausprobieren)


### Stringverarbeitung (Grundlagen)
Python bietet eine Vielzahl von Funktionen zur Verarbeitung von Strings. Hier sind einige grundlegende Beispiele:

Strings können addiert (*konkateniert*) werden.
```python
print("Take this!" + " And that!")
```
Ergibt:
```
Take this! And that!
```

Es ist jedoch nicht möglich, Strings und Zahlen ohne explizite Umwandlung zu addieren:
<!--pytest-codeblocks:expect-error-->
```python
"100" + 5  # => TypeError
```
Macht Sinn.

Aber Moment! Woher weiß Python überhaupt was ein `integer (int)`, ein `float`, or ein `string (str)` ist? Wir haben es doch nirgendwo vorher definiert.

## Duck Typing

Python nutzt ein Prinzip namens **duck typing**, bei dem der Datentyp automatisch zugewiesen wird, basierend auf den vorliegenden Werten (ausser wir legen gezielt etwas anderes fest). Duck Typing heißt das Ganze wegen einem alten Romanzitat:
(*"When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck."*, see [Duck test (wikipedia](https://en.wikipedia.org/wiki/Duck_test#History)))

## Datentypen ändern
Es gibt Situationen, in denen wir explizit einen Datentyp ändern möchten, z. B. von `float` zu `int`:
```python
print(5 + int(7.00001))  # 12
```
ergibt:
```bash
12
```

Nur Vorsicht! Dies ist keine Rundung, sondern ein Abschneiden der Nachkommastellen:
```
>>> int(12.9)
12
```
(Wenn gerundet werden soll, bitte `int(round(12.9))`nutzten)

Zahlen können auch aus Strings erstellt werden:
```
>>> 5 + int("19")
24
```

Aber bitte keine Magie erwarten an dieser Stelle. Python erkennt hier nur offensichtliche Zahlen im String, nicht sowas wie:
<!--pytest-codeblocks:expect-error-->

```python
int("seven")  # => ValueError: invalid literal for int() with base 10: 'seven'
```

Oft wollen wir auch umgekehrt eine Zahl in einen String umwandeln:
```
>>> str(5) + "19"
'519'
```

Das geschieht sehr häufig, oft um einen String irgendwo auszugeben:
```python
print("7 + 5 = " + str(7 + 5))
```
das gibt aus:

```
7 + 5 = 12
```
Da dies so häufig nötig ist, gibt es dafür auch eine Reihe verschiedener Methoden. Die verbreitetste und gleichzeitig die einfachste sind dier sogenannten "f-strings", die euch ein `f"..."` vor den Ausführungszeichen markiert werden. Innerhalb diesen f-Strings werden Werte, egal on Zahlen, Strings oder viele andere Datentypen, die in geschweiften Klammern stehen, automatisch als Strings eingefügt. Zum Beispiel: 
```python
print(f"This is {8} times better than that.")
```
Gibt:
```
This is 8 times better than that.
```
Damit werden wir in der Praxis sehr häufig arbeiten.


### Listen / Sequenzen

**Sequenzen** sind bei Python eine ganze Kategorie verschiedener Datentypen. Ein Beispiel davon hatten wir bereits: **Strings**

Strings sind Sequenzen aus Zeichen (oder Zeichenketten). Jedes einzelne
Zeichen eines strings kann in Python über einen Index angesprochen werden.

```python 
s = "Dies ist ein string"

print(s[0]) # => D
print(s[-1]) # => g
print(len(s)) # => 19
```

## Lists

Auch Listen sind Sequenzen und einer der zentralen Datentypen in Python. Eine Liste (`list`) ist in Python eine Sammlung von Elementen. Eine Liste wird einfach mit eckigen Klammern erstellt.

```python 
fruit = ["apple", "orange", "banana", "pear"]
```

Listen sind ebenfalls Sequenzen und einzelne Elemente können in Python
über Indices adressiert werden.


> ### Mini Quiz: Einmal Raten bitte
>
> Was gibt `print(fruit[1])` aus?  
>
> 1) SyntaxError  
> 2) apple  
> 3) orange  
>
> Was gibt `print(len(fruit))` aus?  
>
> 1) 1  
> 2) 4  
> 3) 21  

```python 
fruit = ["apple", "orange", "banana", "pear"]
print(fruit[2])  # => banana
```

Listen können völlig verschiedene Datentypen enthalten:

```python 
my_list = ["a string", 52, -0.014, "another string"]
print(my_list[0])  # => a string
```

Listen können sogar auch weiter Listen enthalten:

```python 
my_list = [3, [7, 2], "example"]
print(my_list[1])  # => [7, 2]
```

### Tuples

Ein Tuple ist in Python eine andere Form der Sammlung. Anders als `list`
wird ein Tuple mit runden Klammern erstellt. Die Elemente eines Tuples sind -genau wie Listen-
über Indices adressierbar:

```python 
fruit_tuple = ("apple", "orange", "banana", "pear")
print(fruit_tuple[2])  # => "banana"
```

Sieht eigentlich alles aus wie bei der Liste.  
Was ist jetzt da der Unterschied zur Liste?  
In Python unterscheiden wir veränderbare (**mutable**) und unveränderbare
(**immutable**) Datentypen. Listen zählen zu den veränderbaren, Tuple dagegen
sind unveränderbar.

```python 
fruit = ["apple", "orange", "banana", "pear"]
fruit[-1] = "mango"
print(fruit)  # => ['apple', 'orange', 'banana', 'mango']
```

<!--pytest-codeblocks:expect-error-->

```python 
fruit_tuple = ("apple", "orange", "banana", "pear")
fruit_tuple[-1] = "mango"  # => TypeError: 'tuple' object does not support item assignment
```

In einer Liste können wir also z.B. ein Element einfach ersetzen, bei 
einem Tuple geht das ganze aber nicht.

### Sequenzen

Tuple, list, und string sind alles Sequenzen. Damit gibt es eine ganze
Reihe von Funktionen (Methoden) die für alle drei Datentypen gleichermassen
eingesetzt werden können:

| Operation    | Ergebnis                                                     |
| ------------ | ------------------------------------------------------------ |
| x in s       | True wenn ein Element mit dem   Wert x in der Sequenz s enthalten ist, sonst False. |
| x not in   s | False   wenn ein Element mit dem Wert x in der Sequenz s enthalten ist, sonst True. |
| s + t        | Konkatenation der beiden   Sequenzen s und t.                |
| s * n        | n Kopien der Sequenz s werden hintereinandergehängt.         |
| s[i]         | Wiedergeben des i-ten Elementes   von s.                     |
| s[i:j]       | Wiedergabe des Ausschnitts   (slice) von s, der vom i-ten bis zum j-ten Element (nicht einschließlich)   geht. |
| s[i:]        | Wiedergabe des Ausschnitts   (slice) von s, der vom i-ten bis zum letzten Element geht. |
| len(s)       | Gibt die Länge der Sequenz s   wieder.                       |
| min(s)       | Gibt das kleinste Element der   Sequenz s wieder.            |
| max(s)       | Gibt das größte Element der   Sequenz s wieder               |

Ein Beispiel:

```python 
my_list = [1, 4, 6, 20, 11, 99, 13, 1050]
my_str = "Ein String ist auch eine Sequenz."

4 in my_list # => True
"z" in my_str # => True

my_list[1:3] # => [4, 6]
my_str[1:3] # => in
```

#### Slicing!

Eine der wichtigsten Techniken im Umgang mit Sequenzen in Python ist das sogenannte **slicing** (also deutsch in etwa: schneiden). Damit ist gemeint, dass wir aus Sequenzen bestimmte Teilmengen auswählen können.

Dies wird in Python in eckigen Klammern und mit Hilfe von Integern und Doppelpunktzeichen angegeben.
Es können dabei bis zu drei Werte angegeben werden in der Form:

```python
my_variable[start:stop:stepsize]
```

Hier eine Beispiele zur Verdeutlichung:

```python
fruits = ["apple", "banana", "mango", "melon",
          "lemon", "pear", "pineapple"]

# Alle Früchte ab dem Index 2
print(fruits[2:])

# Alle Früchte bis zum Index 3
print(fruits[:3])

# Alle Früchte mit Index 1 bis 6
print(fruits[1:6])

# Jede zweite Frucht mit Index 1 bis 6
print(fruits[1:6:2])  # 2 ist hier die "Schrittweite"

# Jede dritte Frucht
print(fruits[::3])

# Alle Früchte in umgekehrter Reihenfolge
print(fruits[::-1])
```

Typischerweise ist diese Notierung am Anfang etwas gewöhnungsbedürftig wird in Python aber so häufig verwendet, dass man sich das Slicing in der Regel schnell antrainiert. Es gibt z.B. oft auch andere Wege um die Elemente einer Sequenz in umgekehrter Reihenfolge auszugeben. In der Praxis fällt den meisten Entwickler\*Innen aber am schnellsten die Lösung mit `[::-1]` ein, so dass diese sehr oft in Programmen auftaucht.

### List Methoden

Darüber hinaus haben die verschiedenen Datentypen aber noch weitere eigene Methoden
Das Arbeiten mit Strings werden wir in einigen der folgenden Vorlesungen noch
genauer betrachten. Hier aber schon einmal eine Tabelle mit wichtigen Methoden
für Python Listen.

| Operation              | Ergebnis                                                     |
| ---------------------- | ------------------------------------------------------------ |
| my_list[i]   = x       | Das Element mit Index i wird   durch x ersetzt.              |
| my_list.append(x)      | An die Liste s wird ein neues   Element x angehängt.         |
| my_list.extend(t)      | Die Liste my_list   wird um die Element der Sequenz t verlängert. |
| my_list.count(x)       | Gibt die Anzahl der   Listenelemente mit dem Wert x zurück.  |
| del my_list   [i]      | Das Element mit Index i wird aus   der Liste s entfernt, damit veringert   sich auch die Länge der Liste um 1. |
| my_list.index(x)       | Gibt den kleinsten Index von my_list   zurück an dem ein Element gleich x ist. |
| my_list.insert(i,   x) | Falls i >= 0 wird x vor dem   Index i in die Liste my_list eingefügt. |
| my_list.remove(x)      | Das erste Element gleich x wird   aus der Liste my_list entfernt. |
| my_list.sort()         | Die Elemente der Liste werden   aufsteigend sortiert         |
| max(s)                 | Gibt das größte Element der   Sequenz s wieder               |

