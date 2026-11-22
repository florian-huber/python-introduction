# Fehler behandeln mit `try` und `except`

Beim Programmieren können Situationen auftreten, in denen eine Operation nicht wie geplant ausgeführt werden kann. Python signalisiert solche Probleme häufig mit einer **Exception** (Ausnahme), zum Beispiel `ValueError`, `IndexError` oder `TypeError`.

Manchmal ist es sinnvoll, eine solche Exception gezielt abzufangen und kontrolliert darauf zu reagieren.

## `try` und `except`

Im `try`-Block steht Code, bei dem eine bestimmte Exception auftreten könnte. Im passenden `except`-Block legen wir fest, was in diesem Fall passieren soll.

```python
text = "zwölf"

try:
    number = int(text)
    print("Zahl:", number)
except ValueError:
    print("Das war keine gültige ganze Zahl.")

print("Das Programm läuft weiter.")
```

`int("zwölf")` würde normalerweise einen `ValueError` auslösen und das Programm an dieser Stelle abbrechen. Hier wird genau dieser Fehlertyp abgefangen.

Mit einem anderen Wert funktioniert derselbe Code ohne Fehler:

```python
text = "12"

try:
    number = int(text)
    print("Zahl:", number)
except ValueError:
    print("Das war keine gültige ganze Zahl.")
```

Der `except`-Block wird nur ausgeführt, wenn im `try`-Block tatsächlich ein passender Fehler auftritt.

## Konkrete Exceptions abfangen

Ein weiteres Beispiel ist ein ungültiger Listenindex:

```python
colors = ["rot", "blau"]

try:
    print(colors[4])
except IndexError:
    print("Diesen Listenindex gibt es nicht.")

print("Hier geht es weiter.")
```

Wir sollten möglichst den **konkreten Fehlertyp** abfangen, den wir tatsächlich erwarten.

Also lieber:

```python
try:
    number = int("abc")
except ValueError:
    print("Ungültige Zahl")
```

als ein pauschales:

```text
except:
    ...
```

Ein nacktes `except:` fängt sehr viele unterschiedliche Fehler ab und kann dadurch auch Programmierfehler verstecken, die wir eigentlich sehen und beheben möchten.

## Mehrere mögliche Fehlertypen

Falls unterschiedliche Fehler sinnvoll unterschiedlich behandelt werden sollen, können mehrere `except`-Blöcke verwendet werden:

```python
values = [10, 20, 30]
index = 1
divisor = 0

try:
    result = values[index] / divisor
    print(result)
except IndexError:
    print("Der Index existiert nicht.")
except ZeroDivisionError:
    print("Division durch 0 ist nicht möglich.")
```

Welche Exception hier auftritt, hängt davon ab, welche Operation zuerst fehlschlägt.

## `if` oder `try`?

Nicht jede Situation braucht `try` und `except`.

Wenn sich eine Bedingung einfach vorher prüfen lässt, ist ein normales `if` häufig gut lesbar:

```python
numbers = [10, 20, 30]
index = 5

if index < len(numbers):
    print(numbers[index])
else:
    print("Index zu groß.")
```

`try`/`except` ist besonders nützlich, wenn eine Operation selbst fehlschlagen kann und wir gezielt mit diesem Fehler umgehen möchten.

**Wichtig:**

- Nur den Code in `try` schreiben, bei dem die erwartete Exception auftreten kann.
- Möglichst konkrete Exceptions abfangen.
- Exceptions nicht einfach stillschweigend ignorieren.
- `try`/`except` ist kein Ersatz für jede normale `if`-Bedingung.

---

## Zusatz: Fehler mit `raise` bewusst auslösen

Exceptions entstehen nicht nur automatisch. Wir können sie auch selbst auslösen.

Dafür gibt es `raise`.

Angenommen eine Funktion soll nur positive Preise akzeptieren:

```python
def price_with_tax(price, tax_rate):
    if price < 0:
        raise ValueError("price darf nicht negativ sein")

    return price * (1 + tax_rate)


print(price_with_tax(100, 0.19))
```

Bei einem ungültigen Wert wird bewusst ein `ValueError` erzeugt:

<!-- pytest-codeblocks:expect-error -->

```python
def price_with_tax(price, tax_rate):
    if price < 0:
        raise ValueError("price darf nicht negativ sein")

    return price * (1 + tax_rate)


price_with_tax(-20, 0.19)
```

`raise` ist sinnvoll, wenn eine Funktion mit bestimmten Eingaben nicht sinnvoll weiterarbeiten kann.

Der aufrufende Code kann diese Exception wiederum abfangen:

```python
def price_with_tax(price, tax_rate):
    if price < 0:
        raise ValueError("price darf nicht negativ sein")

    return price * (1 + tax_rate)


try:
    result = price_with_tax(-20, 0.19)
except ValueError as error:
    print("Fehler:", error)
```

Mit

```text
except ValueError as error:
```

bekommen wir zusätzlich Zugriff auf das Exception-Objekt und damit zum Beispiel auf die Fehlermeldung.

## Zusatz: Annahmen mit `assert` prüfen

Mit `assert` können Entwicklerinnen und Entwickler Annahmen über den eigenen Programmzustand überprüfen.

Ein einfaches Beispiel:

```python
numbers = [2, 4, 6]

doubled = [number * 2 for number in numbers]

assert len(doubled) == len(numbers)
```

Solange die Bedingung `True` ist, passiert nichts.

Ist die Bedingung `False`, wird ein `AssertionError` ausgelöst:

<!-- pytest-codeblocks:expect-error -->

```python
result = 5 + 5

assert result == 11, "Das Ergebnis entspricht nicht unserer Erwartung."
```

Eine optionale Nachricht hinter dem Komma hilft dabei, die verletzte Annahme zu verstehen.

### `assert` ist nicht dasselbe wie normale Eingabeprüfung

`assert` ist vor allem für **interne Annahmen und Entwicklerchecks** gedacht. Assertions können beim Start von Python mit Optimierungsoptionen deaktiviert werden.

Darum sollten wir wichtige Prüfungen von Benutzereingaben oder Daten nicht ausschließlich mit `assert` durchführen.

Für ungültige Funktionsargumente ist zum Beispiel `raise` geeigneter:

```python
def remaining_capacity(total, used):
    if total < 0 or used < 0 or used > total:
        raise ValueError("Ungültige Kapazitätswerte.")

    remaining = total - used

    # Interne Annahme: Nach der Prüfung oben
    # darf remaining nicht negativ sein.
    assert remaining >= 0

    return remaining


print(remaining_capacity(10, 4))
```

Hier haben die beiden Konstruktionen unterschiedliche Aufgaben:

- `raise ValueError(...)` prüft Werte, die von außen an die Funktion übergeben werden.
- `assert ...` dokumentiert und kontrolliert eine Annahme über den internen Programmzustand.

Für den normalen Einstieg reicht zunächst `try` und `except`. `raise` und `assert` werden besonders interessant, wenn Programme größer werden und Funktionen klarere Verträge darüber bekommen, welche Eingaben erlaubt sind und welche Zustände intern gelten sollen.
