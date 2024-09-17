# Datentypen (*data types*)

Datentypen verbinden Werte (oder eben: Daten) und Methoden. Die Methoden legen fest, was mit den Daten gemacht werden darf/kann (und wie das funktioniert).

In Python gibt es eine Reihe verschiedener Datentypen. Hier beginnen wir zum Einstieg erstmal mit Zahlen und Strings.

## Zahlen (*numbers*)

Zahlen und das Rechnen mit Zahlen funktioniert in Python recht intuitiv und einfach, beinahe wie bei einem Taschenrechner.
Bei den Zahlen unterscheiden wir **integer** (`int`) and **float** (von engl.: "floating point number"). Integer sind ganze Zahlen und float sind Kommazahlen. 
(Randbemerkung: da beim Computer gerundet wird entspricht dies aber nicht genau den reelen Zahlen.)

```py
>>> 5
5

>>> 5.01
5.01
```

Vorzeichen gehen so:
```python
# + positive sign
+5  # 5

# - negative sign
-5  # -5
```

Und, wie gesagt, grundlegende Rechenoperationen gehen wie zu erwarten:
```python
# + addition
5 + 7  # 12

# - subtraction
5 - 7  # -2

# * multiplication
5 * 7  # 35

# / division
5 / 2  # 2.5
```

Aber Vorsicht! Division ergibt immer einen `float`.
```python
print(type(5 / 1))
```
ergibt:
```
<class 'float'>
```
Dieses Beispiel zeigt auch das wir den "Datentypen" in Python erfragen können mit`type()`.

## Print und Rechenoperationen verbinden

Wenig verwunderlich, aber es können natürlich auch mehrere Rechenoperationen hintereinander geschrieben werden. Es ist darüber hinaus auch möglich das Ganze dann mit  `print()` auszugeben.
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
**Übrigen:** Es ist völlig in Ordnung Klammen auch dann einzusetzen, wenn sie mathematisch eigentlich gar nicht nötig werden. Oft macht das eine Berechnung einfacher zu lesen (einfacher für den Menschen):

```python
print((22 * 27 * 7) / 2)  # that's completely fine as well!
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

Kleines quiz: Welche der folgenden Befehle gibt keinen Fehler aus?
```
>>> "five" + "five"

>>> "five" + 5

>>> 2 * "five"

>>> "five" - "ive"
```
Sorry, keine Lösung hier... darum bitte selber probieren...


### Stringverarbeitung (basics)
In Python gibt es eine Menge Dinge zu lernen über das Arbeiten mit Strings. Hier wollen wir erstmal nur einige Basics besprechen, weiter Möglichkeiten kommen in einer späteren Vorlesung.

Strings können addiert werden.
```python
print("Take this!" + " And that!")
```
Ergibt:
```
Take this! And that!
```

Doch Addition zwischen Strings und Zahlen funktioniert nicht!
<!--pytest-codeblocks:expect-error-->
```python
"100" + 5  # => TypeError
```
Macht Sinn.

Aber Moment! Woher weiß Python überhaupt was ein `integer (int)`, ein `float`, or ein `string (str)` ist? Wir haben es doch nirgendwo vorher definiert.

Das heißt **duck typing** und meint, das Python automatisch den Datentypen zuweist der am besten passt (ausser wir legen etwas anderes fest). Duck Typing heißt das Ganze wegen einem alten Romanzitat:
(*"When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck."*, see [Duck test (wikipedia](https://en.wikipedia.org/wiki/Duck_test#History)))

## Datentypen ändern
Manchmal möchten wir den Datentypen ändern, z.B. von ein`integer` aus einem `float` erzeugen:
```python
print(5 + int(7.00001))
```
ergibt:
```
12
```
Nur Vorsicht! Das ist nicht das gleiche wir Runden!
```
>>> int(12.9)
12
```
(Wenn gerundet werden soll, bitte `int(round(12.9))`nutzten)

Oder wir wollen eine Zahl aus einem String erstellen:
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
Da dies so häufig nötig ist, gibt es dafür auch eine Reihe verschiedener Methoden. Eine ist mit `.format()`.
```python
print("This is {} times better than that.".format(5))
```
Gibt:
```
This is 5 times better than that.
```
Aber am besten ist eigentlich der sogenannte "f-strings":
```python
print(f"This is {8} times better than that.")
```
Gibt:
```
This is 8 times better than that.
```
Damit werden wir am meisten arbeiten.

Note: Es gibt auch noch eine Reihe anderer Arten dafür, zum Beispiel einige old-school Varianten aus Python 2, so wie: `print("This is %s times better than that" % 5)`, aber damit arbeiten wir hier nicht. Trotzdem könnte sowas in manchen Foren/Tutorials vorkommen.



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

