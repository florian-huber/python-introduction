# Fehler behandeln mit `try` und `except`

Bei Eingaben und Datenzugriffen kann etwas schiefgehen. Bisher haben wir solche Fehler manchmal durch eine Bedingung verhindert, etwa durch `if index < len(liste)` oder `if key in dictionary`. Es gibt aber auch Situationen, in denen wir eine **Ausnahme** (*Exception*) gezielt abfangen möchten.

## `try` und `except`

Im `try`-Block steht Code, bei dem ein Fehler auftreten könnte. Passt der Fehlertyp zu einem `except`-Block, wird dort weitergearbeitet. Danach geht das Programm **nach der gesamten `try`/`except`-Anweisung** weiter und springt nicht zurück zur fehlerhaften Zeile.

```python
text = "zwölf"

try:
    zahl = int(text)  # Kann ValueError auslösen.
    print("Gelesene Zahl:", zahl)
except ValueError:
    print("Das war keine gültige ganze Zahl.")

print("Das Programm läuft weiter.")
```

Für `text = "12"` gelingt die Umwandlung, der `except`-Block wird übersprungen. Für `"zwölf"` wird der `ValueError` abgefangen. `zahl` wird in diesem Fehlerfall **nicht** neu gesetzt. Darum sollte späterer Code nicht einfach davon ausgehen, dass die Umwandlung erfolgreich war.

Wir können auch gezielt einen ungültigen Listenindex behandeln:

```python
farben = ["rot", "blau"]

try:
    print(farben[4])
except IndexError:
    print("Diesen Listenindex gibt es nicht.")

print("Hier geht es weiter.")
```

**Merke:** Möglichst konkrete Fehlertypen wie `ValueError` und `IndexError` abfangen. Ein pauschales `except:` kann Fehler verbergen, die man eigentlich untersuchen sollte. Nicht jede unerwartete Eingabe braucht `try`/`except`: Oft ist eine vorherige Prüfung mit `if` verständlicher.

## Textdateien: String, Leerfall und fehlende Datei

Im Praktikum kann ein **vorgegebener Textloader** zum Einsatz kommen. Eine sinnvolle Schnittstelle ist: `lade_text(dateiname)` liefert den Dateiinhalt als **String** zurück, bei einer fehlenden Datei liefert sie `None`. Ein **leerer Dateiinhalt** ist dagegen der leere String `""` und **nicht** `None`.

Da den beiden bisherigen Skripten noch keine konkrete Loader-Funktion beilag, zeigt der folgende Block eine **kleine Beispielimplementierung** dieser Schnittstelle. Wenn Sie im Praktikum einen Loader erhalten, verwenden Sie dort dessen vorgegebene Funktion. Sie müssen sie nicht selbst nachprogrammieren.

```python
def lade_text(dateiname):
    try:
        with open(dateiname, "r", encoding="utf-8") as datei:
            return datei.read()
    except FileNotFoundError:
        return None
```

Der Loader fängt `FileNotFoundError` **innerhalb** seiner Funktion ab. Deshalb erhält das aufrufende Programm bei fehlender Datei `None`, statt an dieser Stelle abzubrechen.

<!--pytest-codeblocks:expect-error-->
```python
inhalt = lade_text("beispiel.txt")

if inhalt is None:
    print("Datei nicht gefunden")
elif inhalt == "":
    print("Datei vorhanden, aber leer")
else:
    print("Zeichen:", len(inhalt))
    print("Zeilen:", inhalt.splitlines())
```

`beispiel.txt` ist hier ein Beispielname. Die Datei muss zum Testen tatsächlich vorhanden sein oder durch einen vorhandenen Dateinamen ersetzt werden. Die Unterschiede kann man auch ohne Datei gut untersuchen:

```python
fehlend = None
leer = ""
mehrzeilig = "Erste Zeile\nZweite Zeile\n"

print(fehlend is None)        # => True
print(leer == "")             # => True
print(len(mehrzeilig))
print(mehrzeilig.splitlines())  # => ['Erste Zeile', 'Zweite Zeile']
```

Das `\n` gehört beim Einlesen als Zeilenumbruch zum String. Mit `.splitlines()` machen wir daraus eine Liste von Zeilen. Für das Durchlaufen der Zeilen gibt es dann wieder eine normale `for`-Schleife.

> **Kurze Verständnisfrage:** Warum wäre `if not inhalt:` hier ungeeignet, wenn wir zwischen „Datei fehlt“ und „Datei ist leer“ unterscheiden wollen? (Beides wird als `False` ausgewertet.)
