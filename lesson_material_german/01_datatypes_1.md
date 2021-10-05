# Datentypen (*data types*)

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

### Print und Rechenoperationen verbinden

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
```python
"100" + 5  # => TypeError
```
Macht Sinn.

Aber Moment! Woher weiß Python überhaupt was ein `integer (int)`, ein `float`, or ein `string (str)` ist? Wir haben es doch nirgendwo vorher definiert.

Das heißt **duck typing** und meint, das Python automatisch den Datentypen zuweist der am besten passt (ausser wir legen etwas anderes fest). Duck Typing heißt das Ganze wegen einem alten Romanzitat:
(*"When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck."*, see [Duck test (wikipedia](https://en.wikipedia.org/wiki/Duck_test#History)))

### Datentypen ändern
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

Note: Es gibt auch noch eine Reihe ander Arten dafür, zum Beispiel einige old-school Varianten aus Python 2, so wie: `print("This is %s times better than that" % 5)`, aber damit arbeiten wir hier nicht. Trotzdem könnte sowas in manchen Foren/Tutorials vorkommen.
