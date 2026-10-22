# String handling

## (Stringverarbeitung)

Strings sind neben numerischen Datentypen sicher eines der am häufigsten verwendeten Datenformate, nicht nur in Python.
Denken Sie an den Umgang mit Texteingaben, Dateinamen, Kommentaren, textbasierter Suche, aber auch Textdateien etc.

Zu Beginn der Vorlesung haben wir schon einige String-Operationen kennengelernt:

```python
print("dies" + "das")
print(10 * "?")
```

Strings zählen in Python zu den **Sequenzen**, darum können entsprechende Operationen genutzt werden, etwa `in` und `not in`:

```python
print("das" in "dasselbe")    # => True
print("x" not in "abcdefg")  # => True
```

Ein String besteht aus einzelnen Zeichen. Wie bei Listen beginnt die Indizierung bei 0, und negative Indizes zählen vom Ende:

```python
s = "Ja Nee"
print(len(s))  # => 6 (das Leerzeichen zählt mit)
print(s[0])   # => J
print(s[-1])  # => e

for zeichen in s:
    print(zeichen)
```

Ebenso gibt es das **Slicing**, das wir schon bei Listen kennengelernt haben:

```python
print("Ja Nee"[:2])  # => Ja

s = "abcdefgh"
print(s[2:5])    # => cde (Stopgrenze ausgeschlossen)
print(s[:3])     # => abc
print(s[4:])     # => efgh
print(s[::2])    # => aceg
print(s[::-1])   # => hgfedcba
print(s[6:1:-2]) # => gec
```

Wie bei Listen können Start, Stop und Schrittweite angegeben werden. Bei einer negativen Schrittweite wird rückwärts gelesen. Strings sind allerdings **unveränderlich** (*immutable*): Einzelne Zeichen lassen sich nicht direkt überschreiben. Stattdessen erzeugen Operationen einen neuen String.

```python
text = "Haus"
# text[0] = "M"  # Würde einen TypeError auslösen.
neu = "M" + text[1:]
print(text)  # => Haus
print(neu)   # => Maus
```

> ### MINI QUIZ:
>
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
> Und was der folgende?

```python
s = "nichts is gut!"
print(s[-7:])
```

> a) is gut!  
> b) sthcin  
> c) ValueError

## Escape Character – fix the string

Manche Zeichen oder Zeichenkombinationen werden von Python besonders interpretiert.
Zum einen gibt es einige ungültige Kombinationen, zum Beispiel:

<!-- pytest-codeblocks:expect-error-->
```python 
print("Da steht "Stop"!")  # => SyntaxError: invalid syntax
```

In diesem konkreten Fall lässt sich das Problem sogar durch die Verwendung verschiedener Anführungszeichen beheben:

```python
print('Da steht "Stop"!')  # => Da steht "Stop"!
```

Alternativ verwenden wir den Backslash (`\`) gefolgt von dem Zeichen, das wir im String darstellen möchten:

```python
print("Da steht \"Stop\"!")  # => Da steht "Stop"!
print("Ein Backslash geht darum so: \\")
```

Da der Backslash in String-Literalen ein Escape-Zeichen ist, muss er bei klassischen Windows-Pfaden häufig als `\\` geschrieben werden, zum Beispiel:

```python
path = "C:\\User\\Desktop\\"
filename = path + "testfile.txt"
print(filename)
```

Für die eigentliche Arbeit mit Dateipfaden ist später auch `pathlib` nützlich. Hier geht es zunächst darum, den Backslash in **Strings** zu verstehen.

Auch andere Kombinationen mit `\` lösen ein bestimmtes Verhalten aus. Besonders häufig sind `\n` für einen Zeilenumbruch und `\t` für einen Tabulator:

```python
print("Damit können endlich endlos lange\nTexte geschrieben werden")
print("Spalte A\tSpalte B")
print("Zeile eins\nZeile zwei")
```

Manchmal begegnen uns auch `\r` oder `\r\n` (zum Beispiel als Zeilenenden in Textdateien). Mit `repr()` sehen wir die Escape-Schreibweise, statt den Zeilenumbruch direkt auszugeben:

```python
print(repr("eins\nzwei"))
print(repr("eins\r\nzwei"))
```

## String methods

Darüber hinaus gibt es in Python eine große Zahl spezieller **String-Methoden**.
Wir werden nicht alle besprechen, aber die in meinen Augen wichtigsten.

Zur Wiederholung noch einmal kurz, was Sie machen können, wenn Sie die passende Methode nicht mehr parat haben. Das kommt bei Python übrigens auch bei erfahrenen Programmierer*innen ständig vor!

Eine Möglichkeit ist das Nachschlagen in der [Python-Dokumentation](https://docs.python.org/3/library/stdtypes.html#string-methods). Eine andere zugängliche Quelle ist [w3schools.com](https://www.w3schools.com/python/default.asp).

Hier eine Tabelle mit den wichtigsten String-Methoden:

### String-Methoden in Python

| Methode        | Beschreibung                                                 |
| -------------- | ------------------------------------------------------------ |
| `count()`      | Zählt das Vorkommen einer Zeichenfolge im String             |
| `encode()`     | Kodiert einen String zu Bytes, z. B. als UTF-8               |
| `endswith()`   | Prüft, ob der String mit einer Zeichenfolge endet            |
| `index()`      | Gibt die erste Position einer gesuchten Zeichenfolge zurück. Fehler, wenn sie fehlt |
| `islower()`    | Prüft, ob die enthaltenen Buchstaben kleingeschrieben sind   |
| `isupper()`    | Prüft, ob die enthaltenen Buchstaben großgeschrieben sind    |
| `join()`       | Verbindet mehrere Strings mit einem Trennstring              |
| `lower()`      | Gibt eine kleingeschriebene Version zurück                   |
| `strip()`      | Entfernt Leerraum bzw. angegebene Zeichen an beiden Enden    |
| `lstrip()`     | Wie `strip()`, aber nur am Anfang                            |
| `replace()`    | Ersetzt angegebene Zeichenfolgen                             |
| `rstrip()`     | Wie `strip()`, aber nur am Ende                              |
| `split()`      | Zerlegt einen String in eine Liste                           |
| `splitlines()` | Zerlegt einen String an Zeilenumbrüchen                      |
| `startswith()` | Prüft, ob der String mit einer Zeichenfolge beginnt          |
| `upper()`      | Gibt eine großgeschriebene Version zurück                    |

### `.replace()`

Recht häufig wollen wir bestimmte Zeichen, Wörter oder Wortteile ersetzen. Das geht unter anderem mit `.replace()`.

```python
s = "Nicht immer gefallen uns alle Zeichen"
s.replace("i", "!")
print(s)  # => nichts passiert?
```

Die Methoden ändern den ursprünglichen String nicht, sondern geben einen **neuen** zurück. Wollen wir das Ergebnis später verwenden, speichern wir die Rückgabe:

```python
s_new = s.replace("i", "!")
print(s_new)
print(s)  # weiterhin unverändert
```

Es lassen sich auch mehrere Methodenaufrufe kombinieren:
<!-- pytest-codeblocks:cont -->
```python
print(s.replace("i", "!").replace("e", "3"))
```

> ### Mini-Quiz!
>
> Spekulieren Sie: Was gibt der folgende Befehl aus?

```python
print("abc".replace("ab", "cc").replace("c", "x"))
```

> a) ccx  
> b) ab  
> c) xxx  
> d) abx

### `.upper()` und `.lower()`

Damit könnte man jetzt auch zwischen Groß- und Kleinschreibung wechseln, aber dafür gibt es eigene Methoden in Python: `.upper()` und `.lower()`.

Zum Beispiel:

```python
post = "this is not fair"
post_trumpified = post.upper() + "!"
print(post_trumpified)  # => THIS IS NOT FAIR!
```

Und mit `.lower()` wieder in Kleinbuchstaben:
<!-- pytest-codeblocks:cont -->
```python
post_moderated = post_trumpified.lower()
print(post_moderated)  # => this is not fair!
```

Oft wollen wir Texte (oder Strings) in eine bestimmte Form bringen, damit danach weitere Aktionen oder Analysen funktionieren.
Zum Beispiel Benutzereingaben vereinheitlichen:

```python
eingabe1 = " Muster, Markus."  # Leerzeichen und Punkt stören
eingabe2 = "Test, Trude   "   # Gleich mehrere Leerzeichen?
```

`.strip()` wird genutzt, um Leerraum (*whitespace*) an **beiden Enden** eines Strings zu entfernen. Es gibt auch `.rstrip()` und `.lstrip()`, die nur das rechte oder linke Ende betrachten.

```python
print(eingabe1.strip())
print(eingabe2.strip())
```

`.strip()` kann noch mehr: Wir können angeben, **welche Zeichen** entfernt werden dürfen (und auch mehrere gleichzeitig). Das sind einzelne erlaubte Zeichen, keine vollständige Zeichenfolge. Im Inneren eines Strings bleibt alles stehen.

```python
bereinigt1 = eingabe1.strip("!?. ")
bereinigt2 = eingabe2.strip("!?. ")
print(bereinigt1)  # => Muster, Markus
print(bereinigt2)  # => Test, Trude

print("...Hallo!!!".strip("!."))  # => Hallo
print("Hallo, Welt!".strip("!."))  # => Hallo, Welt
```

Hier sollen nur die ausdrücklich genannten Zeichen entfernt werden. Ein pauschales Entfernen **aller** Satzzeichen wäre eine andere Aufgabe. Und auch hier bleibt die ursprüngliche Eingabe unverändert, solange wir nicht das Ergebnis speichern.

### Strings (Texte) teilen

Bei vielen Eingaben, vor allem aber bei längeren Texten/Strings, wollen wir Strings in kleinere, sinnvolle Stückchen teilen.
Dafür gibt es `.split()`. Zum Beispiel bei einer einfachen Eingabe:

```python
name = "Muster, Markus"
pieces = name.split(",")  # Teilt den String bei jedem Komma.
print(pieces)
```

Das gibt eine **Liste** zurück. Das Leerzeichen vor `Markus` ist noch enthalten. Zum Beispiel lässt es sich mit `.strip()` entfernen:

```python
vorname = pieces[1].strip()
print(vorname)  # => Markus
```

Ein anderes Beispiel:

```python
satz = "Viele Sätze haben viele Worte, manchmal sogar Zeichen!"
worte = satz.split(" ")
print(worte)
print(f"Dieser Satz hat {len(worte)} Worte.")
```

**Unterschied zwischen `.split()` und `.split(" ")`:** Ohne Argument werden aufeinanderfolgende Leerraumzeichen (auch Tabs und Zeilenumbrüche) zusammengefasst. Mit einem **expliziten** Trennzeichen können dagegen leere Listeneinträge entstehen:

```python
text = "eins  zwei\t drei"
print(text.split())      # => ['eins', 'zwei', 'drei']
print(text.split(" "))   # => ['eins', '', 'zwei\t', 'drei']
print("a,,b".split(","))  # => ['a', '', 'b']
print("".split())         # => []
print("".split(","))      # => ['']
```

Wenn wir auch bestimmte Satzzeichen entfernen möchten, kann zusätzlich `.replace()` genutzt werden:

```python
satz = "Viele Sätze haben viele Worte, manchmal sogar Zeichen!"
satz_cleaned = satz.replace(",", "").replace("!", "").replace(".", "")
worte = satz_cleaned.split()  # Ohne Argument auch bei mehreren Leerzeichen praktisch.
print(worte)
print(f"Dieser Satz hat {len(worte)} Worte.")
```

Hier werden **nur** Komma, Ausrufezeichen und Punkt entfernt, nicht automatisch andere Satzzeichen.

Oder so etwas:

```python
mail = "trude_97@monty-mail.com"
tag, provider = mail.split("@")
print(tag)
print(provider)
```

Die Umkehridee liefert `.join()`: Mehrere Strings werden mit einem gewünschten Trennzeichen zu einem String verbunden.

```python
namen = ["Muster", "Markus"]
print(", ".join(namen))  # => Muster, Markus

worte = ["Python", "macht", "Spaß"]
satz = " ".join(worte)
print(satz)  # => Python macht Spaß
print(type(satz))  # => <class 'str'>
```

### String abfragen

Python kommt mit vielen Möglichkeiten, String-Inhalte abzufragen (zusätzlich zur Sequenzoperation, die wir schon gesehen hatten, wie etwa `"x" in "xyz"`).

Typische Beispiele wären `.startswith()` und `.endswith()`:

```python
s = "name: Markus"
print(s.startswith("name:"))  # => True
print(s.endswith("."))        # => False
```

Hierbei können auch mehrere Abfragen kombiniert werden:
<!-- pytest-codeblocks:cont -->
```python
if s.startswith(("Name:", "name:", "NAME:")):
    print("Ja, ist wohl ein Namenseintrag")
```

Eine weitere sehr praktische Methode (die es so ähnlich auch für Listen gibt) ist `.count()`, um die Anzahl von Vorkommen zu zählen:

```python
s = "Die meisten Texte haben schon ein paar 'e's."
print(s.count("s"))   # => 3
print(s.count("te"))  # => 2
print(s.count("Te"))  # => 1
```

**Achtung:** `str.count()` zählt das Vorkommen einer **Zeichenfolge**, nicht automatisch vollständige Wörter! Der Unterschied zur Zählung von Einträgen in einer Liste ist wichtig:

```python
text = "Hausaufgabe Haus"
print(text.lower().count("haus"))  # => 2: auch in 'Hausaufgabe'

woerter = text.lower().split()
print(woerter)                # => ['hausaufgabe', 'haus']
print(woerter.count("haus"))  # => 1: genau ein vollständiger Listeneintrag
```

Und `.index()` gibt die Position des ersten Vorkommens zurück:

<!-- pytest-codeblocks:cont -->
```python
index_te = s.index("te")
print(f"Stelle im String: {index_te}")
print(s[index_te])
```

Anders als `in` löst `.index()` einen `ValueError` aus, wenn die gesuchte Zeichenfolge fehlt. Deshalb kann es sinnvoll sein, zuvor die Zugehörigkeit zu prüfen.

### Von Text zu Wortliste und Häufigkeiten

Jetzt können wir mehrere bekannte Konzepte verbinden: Eine Funktion vereinheitlicht einen String und liefert eine **Liste**, eine zweite Funktion zählt die Listeneinträge und liefert ein **Dictionary**.

```python
def text_zu_woertern(text):
    bereinigt = text.lower().replace(",", "").replace(".", "").replace("!", "")
    return bereinigt.split()


def zaehle_woerter(woerter):
    haeufigkeiten = {}
    for wort in woerter:
        if wort in haeufigkeiten:
            haeufigkeiten[wort] += 1
        else:
            haeufigkeiten[wort] = 1
    return haeufigkeiten


def analysiere_text(text):
    woerter = text_zu_woertern(text)    # str -> list
    return zaehle_woerter(woerter)      # list -> dict


text = "Tee, Kaffee, Tee! Wasser."
woerter = text_zu_woertern(text)
print(woerter)           # => ['tee', 'kaffee', 'tee', 'wasser']
print(type(woerter))     # => <class 'list'>

haeufigkeiten = zaehle_woerter(woerter)
print(haeufigkeiten)     # => {'tee': 2, 'kaffee': 1, 'wasser': 1}
print(type(haeufigkeiten))  # => <class 'dict'>

print(analysiere_text(text))
print(analysiere_text(""))  # => {}
```

Jede Funktion lässt sich **einzeln** prüfen. `analysiere_text()` verbindet beide Schritte. Die Zwischenergebnisse haben unterschiedliche Typen. Auch hier gilt: Wir entfernen bewusst nur die drei angegebenen Satzzeichen. Für eine allgemeine Textanalyse wären weitere Regeln nötig.

### Encodings (Zeichenkodierungen)

Zeichen (*characters*) können auf einem Rechner auf verschiedene Arten kodiert werden. **Unicode** beschreibt einen großen Zeichenvorrat und ordnet Zeichen Nummern zu. Dabei ist **UTF-8** eine sehr verbreitete Kodierung dieses Zeichenvorrats in Bytes. **ASCII** ist ein älterer, kleinerer Zeichensatz, dessen Zeichen auch in Unicode enthalten sind.

Für den Moment werden wir damit nicht mehr machen. Sollten Sie beim Lesen oder Verarbeiten einer Textdatei einmal auf einen `UnicodeDecodeError` stoßen, wissen Sie jetzt wenigstens, unter welchem Schlagwort Sie weitersuchen können.