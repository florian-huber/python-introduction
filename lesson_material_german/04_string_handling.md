# String handling
## (Stringverarbeitung)

Strings sind neben numerischen Datentypen sicher eines der am häufigsten verwendeten Datenformate in Python.
Denken Sie an den Umgang mit Texteingaben, Dateinamen, Kommentaren, Text-basierten Suchen, aber auch Textdateien etc.

Zu Begin der Vorlesung haben wir schon einige String-Operationen kennengelernt:

```python 
print("dies" + "das")
print(10 * "?")

```
Strings zählen in Python zu den Sequenzen, darum können demensprechende Methoden genutzt werden, etwa:
```python 
print("das" in "dasselbe")  # => True
print("x" not in "abcdefg")  # => True
print("aabbccdd".count("a"))  # => 2

```
Ebenso das **slicing**:
```python 
print("Ja Nee"[:2])  # => Ja
```

> ### MINI QUIZ:
> Nur zur Wiederholung: Was gibt der folgende Befehl aus?
```python 
s = "alles gut"
print(len(s))
```
> a) ValueError  
> b) 8  
> c) 9  
> d) alles gut
>  
> Und was der Folgende:  
```python 
s = "nichts is gut!"
print(s[-7:])
```
> a) is gut!  
> b) sthcin  
> c) ValueError

## Escape Character - fix the string
Manche Zeichen oder Zeichenkombinationen werden von Python besonders interpretiert.
Zum einen gibt es einige unmögliche Kombinationen, z.B. 

<!--pytest-codeblocks:expect-error-->
```python 
print("Da steht "Stop"!")  # => SyntaxError: invalid syntax

```
Das kann behoben werden durch die Verwendung vom backslash ("\") gefolgt durch das gewünschte Zeichen.
```python 
print("Da steht \"Stop\"!")  # => Da steht "Stop"!
print("Ein backslash geht darum so: \\")
```

Auch andere Kombinationen mit "\" lösen bestimmtes Verhalten aus, am häufigsten benutzt davon ist wahrscheinlich "\n" was einen Zeilenumbruch bedeutet.
```python 
print("Damit können endlich endlos lange \n Texte geschrieben werden")
```
```python 
print("Manchmal wird '\\r' oder '\\r\\n' \r\n benutzt statt '\\n' \n doch",
      "wir nehmen hier eigentlich immer nur '\\n' \n",
      "wichtig ist dann noch \t tab.")
```

## String methods
Darüber hinaus gibt es aber in Python noch eine grosse Zahl von speziellen **String-Methoden**.
Wir werden nicht alle besprechen, aber die in meinen Augen Wichtigsten.
Zur Wiederholung nochmal kurz was sie machen können wenn sie die passende
Methode nicht mehr parat haben (das kommt bei Python übrigens auch bei
erfahrenen Programmierer*Innen ständig vor, zumindest bei mir...)

### .replace()
```python 
s = "Nicht immer gefallen uns alle Zeichen"
s.replace("i", "!")
print(s)  # => nichts passiert?

```
Die Methoden ändern nicht den original-String, sondern geben einen neuen zurück
<!--pytest-codeblocks:cont-->
```python 
s2 = s.replace("i", "!")
print(s2)


```
Es lassen sich auch mehrere Methodenaufrufe kombinieren:
<!--pytest-codeblocks:cont-->
```python 
print(s.replace("i", "!").replace("e", "3"))

```
> ### Mini Quiz!
> Spekulieren Sie: Was gibt der folgende Befehl aus?
```python 
print("abc".replace("ab", "cc").replace("c", "x"))
```
> a) ccx b) ab c) xxx d) abx


### .upper() und .lower()
Damit könnte man jetzt auch z.B. zwischen Gross und Kleinschreibung wechseln,
aber dafür gibt es eigene Methoden in Python --> .upper() und .lower()"
Zum Beispiel sowas:
```python 
tweet = "this is not fair"
tweet_trumpified = tweet.upper() + "!"
print(tweet_trumpified)  # => THIS IS NOT FAIR!

```
Und mit .lower() wieder halbwegs zurück:
<!--pytest-codeblocks:cont-->
```python 
tweet_moderated = tweet_trumpified.lower()
print(tweet_moderated)


```
Oft wollen wir Texte (oder Strings) in eine bestimmte Form bringen damit danach weitere Aktionen oder Analysen funktionieren.
Zum Beispiel User-Eingaben vereinheitlichen:
```python 
eingabe1 = " Muster, Markus."  # Leerzeichen und Punkt stören
eingabe2 = "Test, Trude   "  # Gleich mehrere Leerzeichen?

```
`.strip()` wird genutzt um Leerzeichen (white spaces) an beiden Enden eines Strings zu entfernen. Es gibt auch `. rstrip()` und `.lstrip()` die nur auf das rechte oder linke Ende schauen.
<!--pytest-codeblocks:cont-->
```python 
print(eingabe1.strip())
print(eingabe2.strip())
```

`.strip()` kann noch mehr. Es können nämlich auch weitere Zeichen mit angegebn werden die entfernt werde sollen (und auch mehrere gleichzeitig!)
<!--pytest-codeblocks:cont-->
```python 
print(eingabe1.strip("!?. ")) 
print(eingabe2.strip("!?. "))
```

### Strings (Texte) teilen
Bei vielen Eingaben, v.a. aber natürlich bei längeren Texte/Strings wollen wir die Strings in kleiner, sinvolle Stückchen teilen.
Dafür gibt es `.split()` Z.B. bei einer einfachen Eingabe wie:
```python 
name = "Muster, Markus"
pieces = name.split(",")  # Teilt den String bei jedem ','
print(pieces)
```
Das gibt eine Liste zurück mit den einzelnen Elementen.

Ein anderes Beispiel:
```python 
satz = "Viele Sätze haben viele Worte, manchmal sogar Zeichen!"
worte = satz.split(" ")
print(worte)
print(f"Dieser Satz hat {len(worte)} Worte.")
```

Oder sowas:
```python 
mail = "trude_97@monty-mail.com"
tag, provider = mail.split("@")
print(tag)
print(provider)
```

### String abfragen
Python kommt mit vielen Möglichkeiten um String-Inhalte abzufragen.
(zusätzlich zu der Sequenzmethode die wir schon gesehen hatten wie: `"x" in "xyz"`)

Typische Beispiele wären `.startswith()` und `.endswith()`:
```python 
s = "name: Markus"
s.startswith("name:")  # => True
s.endswith(".")  # => False

```

Hierbei können auch mehrere Abfragen kombiniert werden
<!--pytest-codeblocks:cont-->
```python 
if s.startswith(("Name:", "name:", "NAME:")):
    print("Ja, ist wohl ein Namenseintrag")

```
Eine weitere sehr praktische Funktion (die es so ähnlich auch für Listen gibt)
ist `.count()` um die Anzahl eines Elementes zu zählen.
```python 
s = "Die meisten Texte haben schon ein paar 'e's."
print(s.count("s"))  # => 3
print(s.count("te"))  # => 2
print(s.count("Te"))  # => 1

```
Und `.index()` gibt die Position des ersten Elements zurück
<!--pytest-codeblocks:cont-->
```python 
index_te = s.index("te")
print(f"Stelle im String: {index_te}")
print(s[index_te])

```
### Encodings (Zeichen-Kodierungen)
(wurde schon beim Lesen/Schreiben von Dateien erwähnt, nicht Prüfungsrelevant)
Zeichen (characters) können auf dem Rechner auf verschiedene Arten kodiert werden. Der typische Standard heisst **Unicode** und die bei weitem am häufigsten Vorkommende Variante ist `utf-8`. In Python findet sich aber auch noch oft **ASCII**.

Für den Moment werden wir damit nicht mehr machen.
Aber sollten Sie beim Lesen oder Hantieren eines Strings auf den Error `UnicodeDecodeError` stossen, dann wissen sie wenigstens unter welchem Schlagwort Sie weitersuchen können.
