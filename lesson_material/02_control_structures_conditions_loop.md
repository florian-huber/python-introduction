## Wiederholung -> Datentypen, Syntax, warum Python

# Skripte/Programme
Letztes Mal v.a. kurze Befehle, die wir entweder im Terminal
oder über eine integrierte Entwicklungsumgebung (IDE) eingegeben haben.

+ Programm $\approx$ Skript (und Programm > Skript)

Dies ist nur ein kleines Skript
```python 
fruit = ["apple", "orange", "banana", "pear"]
juice_choice = 1
ice_choice = 2
print(fruit[juice_choice] + " juice")  # oder: print(f"{fruit[juice_choice]} juice")
print(fruit[ice_choice] + " ice cream")
```
> ### Mini Quiz
> Was gibt dieses Skript aus?
> 1) apple juice, orange ice cream 
> 2) orange juice, banana ice cream
> 3) banana juice, orange ice cream
> 4) Keine Ahnung! Ich müsste raten (will aber nicht)


Mhh. Ziemlich langweiliges Programm
Was braucht man häufig um interessantere Dinge umzusetzen?

Zum einen: **Input**
Dazu kommen wir aber im Detail etwas später.
Für den Anfang nur eine einfache Input-art:
    
<!--pytest-codeblocks:expect-error-->
```python 
fruit = ["apple", "orange", "banana", "pear"]
print(f"We have: {fruit}")
fruit_choice = input("Please enter your choice: ")
print(f"Here is your {fruit_choice} ice cream!")
```
> ### Mini Quiz:
> Aber was passiert wenn ich "Ketchup" eingebe?  
> a) Here is your Ketchup ice cream!  
> b) ValueError  
> c) We have no Ketchup ice cream!  


Das führt uns direkt zur Frage: 
Wie überprüfen wir ob es unsere Wahl überhaupt gibt?
Bei den Sequenzen hatten wir schon kurz die Abfrage `xxx in my_list`
Das ist bereits eine Bedingung und die werden in Python auf verschiedene Arten abgefragt:


## Bedingungen

In vielen Programmen wollen wir Entscheidungen treffen was als nächstes geschieht.
Das wird v.a. über das Abfragen von Bedingungen gemacht.
Im Prinzip ist dies nichts anderes als eine einfache Frage die eindeutig
mit *wahr* (`True`) oder *falsch* (`False`) beantworted werden kann.

```python 
9 < 10  # => True

print(3 < 4 < 5)  # => True

print(4.0 == 4)  # => True

print(4.01 == 4)  # => False

"apple" < "banana"  # => True (durch die Reihenfolge im Alphabet sagt Python: a < b)

"apple" != "pear"  # => True
```
### Notiz: `is` und `==` bedeuten verschiedene Dinge
Vorsicht mit dem `is`. Dem normalen Sprachgebrauch nach scheint es völlig Sinn
zu machen wenn wir schreiben: 
```python 
a = 12345678
b = 12345678
print(a is b)  # => False
```
Dies gibt aber nicht das gleiche aus wie `==`:
```python 
print(a == b)  # => True
```
`is` bezieht sich in Python auf eine Abfrage die darauf zielt zu sehen
ob es sich um ein und Dasselbe Objekt handelt. Genauer eigentlich, ob 
zwei Objekte gleich sind und an der selben Speicheradresse hinterlegt sind.
Dies kann auch über `id()` (von identity) ausgegeben werden
```python 
print(id(a))
print(id(b))
print(id(a) != id(b))  # => True
```
### Optional\*: lazy Python...
\* *Optional heisst auch immer, "nicht Prüfungsrelevant"*  
Bei "kleinen" Objekten kann es allerdings passieren, dass diese doch die gleiche 
identity bekommen. Das macht es leider noch ein wenig unübersichtlicher
```python 
a = "looks like the same string"
b = "looks like the same string"
print(a is b)  # => True or False (depends on where it is run...)

a = 5
b = 5
a is b  # => True
```
aber:
```python 
a = 30019
b = 30019
a is b  # => False !?! Also besser nicht so benutzen...
```
### Wichtig zum Thema `is` vs `==` is v.a.:
+ Alle Datentypen ausser Zahlen, Strings, Bool und None erzeugen neue Objekte
+ `is` benutzen um nach identischen Objekten zu fragen
+ `==` benutzen um nach gleichem "Inhalt" zu fragen
Beispiele wofür es gedacht ist:

```python 
a = [1, 2]
b = [1, 2]
print(a is b)  # => False
```
oder:

```python 
a = [1, 2, 3, 4]
b = a
print(a is b)  # => True

b[0] = 77
print(a)  # => [77, 2, 3, 4]
```
Wie gesagt, Wir hatten auch letztes Mal schon eine Bedingung bei den Sequenzen
```python 
2 in [1, 2, 3, 4, 5]  # => True
2 not in [1, 2, 3, 4, 5]  # => False 
"z" not in "Ein String ohne kleines Z"  # => True
```
logische Bedingungen and, or, not

```python 
4 < 1 or 5 > 3  # => True
```
besser lesbar:
```python 
(4 < 1) or (5 > 3)  # => True

True and False  # => False
True or False  # => True

s = "ja"
(s == "Ja") or (s == "JA") or (s == "ja")  # => True
```
***Vorsicht:*** Logik-Abfragen können schnell kompliziert werden!
 
### Bedingte Anweisungen (if, if-else)
Wofür jetzt das Ganze? 
Konditionen und logische Abfragen sind essentiell wenn es um Programmflüsse geht.
Konkret geht es hier um sogenannte "bedingte Anweisungen".

```python 
number = 0.01
if number >= 0:
    print(f"{number} ist positiv.")
```
Als Beispiel können wir nun das Früchte-Skript von vorhin noch einmal
überarbeiten.
<!--pytest-codeblocks:expect-error-->
```python 
fruit = ["apple", "mango", "banana", "pear"]
print(f"We have: {fruit}")
fruit_choice = input("Please enter your choice: ")
if fruit_choice in fruit:
    print(f"Here is your {fruit_choice} ice cream!")
else:
    print(f"{fruit_choice}? We don't have it.")
```
***Wichtig:*** In Python wird mit Einrückungen gearbeitet! 
(andere Sprachen nutzen dafür oft Klammern)
Die Art der Einrückungen kann im Prinzip frei gewählt werden, sie muss
nur Konsistent sein. Meistens werden als Standard aber 4 Leerzeichen gewählt.


Weitere Ergänzung des Skripts: Was passiert z.B. mit Apple
<!--pytest-codeblocks:expect-error-->
```python 
fruit = ["apple", "mango", "banana", "pear"]
print(f"We have: {fruit}")
fruit_choice = input("Please enter your choice: ")
if fruit_choice == "mango":
    print("Oh, I just see that we are out of mango!")
elif fruit_choice in fruit:
    print(f"Here is your {fruit_choice} ice cream!")
else:
    print(f"{fruit_choice}? We don't have it.")
```
Was wir an dem Beispiel gesehen haben waren also neben `if ...Bedingung... :`
auch `if-else`:
```python 
number = 0.01   

if number >= 0:
    print(f"{number} ist eine positive Zahl.")
else:
    print(f"{number} ist eine negative Zahl.")
    
```
Und `if-elif-else`: 
```python 
number = 0.01

if number > 0:
    print(f"{number} ist eine positive Zahl.")
elif number < 0:
    print(f"{number} ist eine negative Zahl.") 
else:
    print(f"{number} müsste 0 sein?")
```
Damit kann man ewig lange Abfragen bauen
Z.B. sowas: 
<!--pytest-codeblocks:expect-error-->
```python 
angebot = float(input("Dein Angebot: "))
if angebot <= 0:
    print("Richtig witzig, danke.")
elif angebot < 10:
    print("Lächerlich!")
elif angebot < 20:
    print("Nein, lieber nicht.")
elif angebot < 25:
    print("Das kommt schon in die Nähe...")
elif angebot < 30:
    print("Mmhhh. OK. Weil du's bist.")
elif angebot < 40:
    print("Ok. Deal.")
elif angebot < 100:
    print("Deal. Eine Freude mit dir Geschäfte zu machen.")
else:
    print("Moment... irgendwas stimmt hier nicht...")
```
### While loop (While-Schleife)
```python 
x = 2
while x < 1000:
    x = x**2
    print(x)
    
```
Anderer Startwert:
```python 
x = 1.0000002
while x < 1000:
    x = x**2
    print(x)
```
**Vorsicht:** While loops können auch endlos laufen!
=> Mit Ctrl+C kommt ihr da aber wieder raus :)
```python 
x = 0.9
while x < 1000:
    x = x**2
    # print(x) 
```
Ein `while` Loop lässt sich auch mit `else` kombinieren:
```python 
x = 2
while x < 100:
    x = x + 2 * x
    print(x)
else:
    print("Fertsch!")
    
```
Das ist aber (in diesem Fall) vom Ablauf des Programmes her nichts anderes als folgendes:
```python 
x = 2
while x < 100:
    x = x + 2 * x
    print(x)
print("Fertsch!")
```
### For loops (For Schleifen)
In der Praxis werden in Python sogenannte "for loops" deutlich häufiger als
"while loops" genutzt. 
Vom Prinzip her sieht das Ganze so aus.

<!--pytest-codeblocks:expect-error-->
```python 
for expression in iterable:
    code...
else:
    other code...
```
Wir lassen den `else` Teil erstmal weg. 
Im Klartext macht ein `for` loop das Folgende. Der Loop läuft einmal durch alle
Elemente aus `iterable`. Ein Beispiel:
```python 
for antwort in ["ja", "nee", "vielleicht", "weiss nicht"]:
    print(f"Ich sag mal...{antwort}")
```
Das Funktioniert mit jeder Python Sequenz auf diese Art:
```python 
for char in "That's a string and full of letters.":
    print(char)

for x in [0, 2, 19, 3]:
    print(x + 1)
```
### Best friends: `range` and `for`
Oft möchten wir eine Programmteil n-mal laufen lassen. Dafür nutzt man in 
Python `range`. `range` ist ein "generator", einen Funktionstyp den wir später
noch genauer besprechen. An dieser Stelle reicht es zu wissen, dass es eine
Funktion ist, die Werte erst ausgibt, wenn sie ausgelesen werden.
`range` generiert Integer ein einem gewünschten Intervall und Abstand
```python 
print(list(range(5)))  # => [0, 1, 2, 3, 4]
print(list(range(4, 8)))  # => [4, 5, 6, 7]
print(list(range(10, 30, 4)))  # => [10, 14, 18, 22, 26]
```
> ### Mini Quiz
> Was gibt `list(range(-3, 3)` aus?  
> a) [-3, 0, 3]  
> b) [-3, -2, -1, 0, 1, 2, 3]  
> c) [-3, -2, -1, 0, 1, 2]  


OK, zurück zum eigentlichen Einsatz, dem `for` loop:
```python 
for i in range(5):
    print(i)
    
```
Tada!
Falls die Zahl `i` innerhalb der Schleife nicht verwendet wird kann sie auch
weggelassen werden:
```python 
for _ in range(3):
    print("Dreimal: Ja!")
```
Falls man einen Zähler und ein Element aus einer Sequenz benötigt, gibt es
die Alternative `enumerate` statt `range`:
```python 
fruits = ["apple", "mango", "banana", "pear"]
for i, fruit in enumerate(fruits):
    print(f"{i + 1}. {fruit}")
```
### Zwei weitere wichtige Optionen in Loops: `break` und `continue`
`break` ermöglicht es aus Loops vorzeitig auszubrechen. Zum Beispiel hier:

```python 
for zeichen in "Ja genau. Dies ist ein string.":
    if zeichen == ".":
        print("Punkt gefunden!")
        break
    # Hier kommt man nur hin wenn break nicht ausgeführt wurde
    print(zeichen)

print("Fertig.")
```
`continue` ist dagegen die Aufforderung den laufenden Zyklus des Loops
zu verlassen, aber dann mit dem Loop fortzufahren.

```python 
zahlen = [2, 3, 5, 9, 557, 1232, 233358]
for zahl in zahlen:
    if zahl%3 != 0:
        continue
    # Hier kommt man nur hin wenn continue nicht ausgeführt wurde
    print(f"{zahl} ist durch 3 teilbar!")
```
Beide Befehle (`continue` und `break`) können genauso auch in `while` Schleifen
eingesetzt werden.
### List comprehension !
In Python gibt es noch eine andere, sehr kompakte Art einen Loop zu bauen.
Das ist die sogenannte "List comprehension".
```python 
my_list = [1, 2, 10]
new_list = [x**2 for x in my_list]
print(new_list)
```
Als `for` loop wäre das ganze deutlich länger:
```python 
my_list = [1, 2, 10]
new_list = []  # leere Liste
for i in my_list:
    new_list.append(i ** 2)
print(new_list)
```
> ### Mini Quiz!
> Was gibt die folgende List comprehension aus?  
> `print([s[0] for s in ["eins", "zwei", "drei"]])  
> a) eins  
> b) eins, zwei, drei  
> c) e, z, d  
> d) 0, 0, 0  


--> go to FUNCTIONS !
