# String handling
## (Stringverarbeitung)

Strings sind neben numerischen Datentypen sicher die am häufigsten verwendeten
Formate.
Denken Sie an den Umgang mit Texteingaben, Dateinamen, Kommentaren, Text-basierten Suchen etc.

Zu Begin der Vorlesung haben wir schon einige String operationen kennengelernt:

```python 
print("dies" + "das")
print(10 * "?")

```
V.a. da strings in Python Sequenzen sind und demensprechende Methoden genutzt werden können
```python 
print("das" in "dasselbe")  # => True
print("x" not in "abcdefg")  # => True
print("aabbccdd".count("a"))  # => 2

```
Ebenso das "slicing"
```python 
print("Ja Nee"[:2])  # => Ja


```
> MINI QUIZ:
> Nur zur Wiederholung: Was gibt der folgende Befehl aus?
> ```python 
> s = "alles gut"
> print(len(s))
> ```
> a) ValueError b) 8 c) 9 d) alles gut
> 
> Und was der folgende:
> ```python 
> s = "nichts is gut!"
> print(s[-7:])
> ```
> a) is gut! b) sthcin c) ValueError


Darüber hinaus gibt es aber in Python noch eine grosse Zahl von speziellen
String-Methoden.
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
Die Methoden ändern nicht den original-String, sondern geben einen neuen zurück.
<!--pytest-codeblocks:cont-->
```python 
s2 = s.replace("i", "!")
print(s2)

```
### .upper() und .lower()
Damit könnte man jetzt auch z.B. zwischen Gross und Kleinschreibung wechseln,
aber dafür gibt es eigene Methoden in Python --> .upper() und .lower()
```python 
tweet = "this is not fair"
tweet_trumpified = tweet.upper() + "!"
print(tweet_trumpified)  # => THIS IS NOT FAIR!

```
```python 
tweet_moderated = tweet_trumpified.lower()
print(tweet_moderated)

```
