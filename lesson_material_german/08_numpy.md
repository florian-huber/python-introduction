# Numerische Daten in Python mit NumPy

`NumPy` ist eine der wichtigsten Python-Bibliotheken für das Arbeiten mit numerischen Daten. Der zentrale Datentyp ist das **NumPy-Array** (`ndarray`). Arrays können Vektoren, Tabellen und auch höherdimensionale Daten darstellen.

Ein großer Vorteil ist, dass viele Rechenoperationen direkt auf ganzen Arrays ausgeführt werden können. Wir müssen dafür nicht jedes Element mit einer eigenen Python-Schleife bearbeiten.

## NumPy importieren und erste Arrays

Üblicherweise wird NumPy so importiert:

```python
import numpy as np
```

Mit `np.array()` können wir aus einer Python-Liste ein NumPy-Array erzeugen:

<!-- pytest-codeblocks:cont -->

```python
numbers = np.array([1, 2, 3, 4])

print(numbers)
print(type(numbers))
```

Ein Array sieht auf den ersten Blick ähnlich wie eine Liste aus. Bei Rechnungen verhalten sich die beiden Datentypen aber unterschiedlich.

```python
import numpy as np

numbers_list = [1, 2, 3]
numbers_array = np.array([1, 2, 3])

print(numbers_list * 2)
print(numbers_array * 2)
```

Die Liste wird wiederholt:

```text
[1, 2, 3, 1, 2, 3]
```

Beim NumPy-Array wird dagegen jedes Element mit `2` multipliziert:

```text
[2 4 6]
```

Genau diese **elementweisen Operationen** sind eine der wichtigsten Ideen bei NumPy.

## Arrays direkt erzeugen

Nicht immer liegen die Daten schon als Liste vor. NumPy bietet verschiedene Funktionen, mit denen Arrays direkt erzeugt werden können.

### `zeros()`, `ones()` und `full()`

```python
import numpy as np

zeros_1d = np.zeros(5, dtype=int)
ones_1d = np.ones(5, dtype=int)
sevens_1d = np.full(5, 7, dtype=int)

print(zeros_1d)
print(ones_1d)
print(sevens_1d)
```

Für zweidimensionale Arrays geben wir die gewünschte Form als Tupel an:

```python
import numpy as np

zeros_2d = np.zeros((2, 3), dtype=int)
ones_2d = np.ones((2, 3), dtype=int)
sevens_2d = np.full((2, 3), 7, dtype=int)

print(zeros_2d)
print(ones_2d)
print(sevens_2d)
```

Mit `dtype=int` legen wir hier ausdrücklich fest, dass ganzzahlige Werte verwendet werden sollen. Ohne diese Angabe erzeugen `zeros()` und `ones()` standardmäßig Gleitkommazahlen.

## Zahlenfolgen mit `arange()`

`np.arange()` funktioniert ähnlich wie `range()`:

```python
import numpy as np

values = np.arange(0, 10, 2)
print(values)
```

Das Ergebnis ist:

```text
[0 2 4 6 8]
```

Wie bei `range()` ist die **Stopgrenze exklusiv**. Die `10` selbst ist also nicht mehr enthalten.

Ein häufiges Muster ist, zunächst eine Folge zu erzeugen und anschließend ihre Form zu ändern:

```python
import numpy as np

values = np.arange(1, 13)
matrix = values.reshape(3, 4)

print(values)
print(matrix)
```

`values` enthält 12 Elemente. Deshalb kann es beispielsweise in `3 * 4`, `4 * 3`, `2 * 6` oder `6 * 2` Elemente umgeformt werden.

Die Anzahl der Elemente muss aber passen:

<!-- pytest-codeblocks:expect-error -->

```python
import numpy as np

values = np.arange(1, 13)
values.reshape(5, 3)
# => ValueError, weil 12 Elemente nicht in eine 5x3-Form passen
```

## Eigenschaften eines Arrays

Für ein Array sind mehrere Eigenschaften wichtig, die leicht verwechselt werden können:

```python
import numpy as np

data = np.array(
    [[10, 20, 30],
     [40, 50, 60]]
)

print("ndim:", data.ndim)
print("shape:", data.shape)
print("size:", data.size)
print("dtype:", data.dtype)
```

Dabei bedeutet:

- `.ndim`: Anzahl der Dimensionen/Achsen
- `.shape`: Länge jeder Dimension
- `.size`: Gesamtzahl aller Elemente
- `.dtype`: Datentyp der Elemente

Für das Beispiel gilt also:

```text
ndim: 2
shape: (2, 3)
size: 6
```

Das Array hat **2 Dimensionen**, **2 Zeilen und 3 Spalten** und insgesamt **6 Werte**.

## Indexing: einzelne Werte auswählen

Wie bei Listen beginnt die Zählung bei Index `0`.

```python
import numpy as np

matrix = np.array(
    [[10, 11, 12, 13],
     [20, 21, 22, 23],
     [30, 31, 32, 33]]
)

print(matrix[0, 0])
print(matrix[1, 2])
print(matrix[2, 3])
```

Bei einem 2D-Array schreiben wir normalerweise:

<!-- pytest-codeblocks:skip -->

```python
matrix[zeile, spalte]
```

Negative Indizes funktionieren ebenfalls:

<!-- pytest-codeblocks:cont -->

```python
print(matrix[-1, -1])
print(matrix[-1, 0])
print(matrix[0, -1])
```

`-1` bezeichnet jeweils den letzten Eintrag entlang der entsprechenden Achse.

## Slicing: Zeilen, Spalten und Teilbereiche

Mit `:` können wir ganze Bereiche auswählen.

Wir verwenden wieder:

```python
import numpy as np

matrix = np.array(
    [[10, 11, 12, 13],
     [20, 21, 22, 23],
     [30, 31, 32, 33]]
)
```

### Eine ganze Zeile

<!-- pytest-codeblocks:cont -->

```python
row = matrix[1, :]
print(row)
print(row.shape)
```

Ergebnis:

```text
[20 21 22 23]
(4,)
```

### Eine ganze Spalte

<!-- pytest-codeblocks:cont -->

```python
column = matrix[:, 2]
print(column)
print(column.shape)
```

Ergebnis:

```text
[12 22 32]
(3,)
```

### Ein rechteckiger Ausschnitt

<!-- pytest-codeblocks:cont -->

```python
block = matrix[1:, 1:3]
print(block)
print(block.shape)
```

Ergebnis:

```text
[[21 22]
 [31 32]]
(2, 2)
```

Versucht bei solchen Ausdrücken möglichst zuerst die **Form des Ergebnisses** vorherzusagen und erst danach die konkreten Werte.

## Rechnen mit Arrays

### Array und Skalar

Eine Rechnung mit einem einzelnen Wert wird elementweise auf das gesamte Array angewendet:

```python
import numpy as np

numbers = np.array([1, 2, 3, 4])

print(numbers + 10)
print(numbers * 3)
print(numbers ** 2)
```

Dafür brauchen wir keine eigene Schleife.

### Zwei gleich geformte Arrays

Auch Rechnungen zwischen zwei Arrays sind elementweise:

```python
import numpy as np

a = np.array([10, 20, 30, 40])
b = np.array([1, 2, 3, 4])

print(a + b)
print(a - b)
print(a * b)
```

Das gleiche funktioniert mit gleich geformten 2D-Arrays:

```python
import numpy as np

a = np.array(
    [[1, 2],
     [3, 4]]
)

b = np.array(
    [[10, 20],
     [30, 40]]
)

print(a + b)
```

Später werden wir noch weitere Regeln für unterschiedlich geformte Arrays kennenlernen. Für den Einstieg arbeiten wir zunächst mit Skalaren oder gleich geformten Arrays.

## Vergleiche erzeugen boolesche Masken

Auch Vergleiche werden elementweise ausgeführt:

```python
import numpy as np

values = np.array([2, 7, 4, 9, 1, 6])

mask = values >= 5

print(mask)
```

Das Ergebnis ist wieder ein Array:

```text
[False  True False  True False  True]
```

Diese Wahrheitswerte können direkt zum Auswählen benutzt werden:

<!-- pytest-codeblocks:cont -->

```python
selected = values[mask]

print(selected)
print(selected.shape)
```

Ergebnis:

```text
[7 9 6]
(3,)
```

Die Auswahl enthält nur noch die Werte, für die die Maske `True` war.

Es kann auch passieren, dass kein Wert ausgewählt wird:

<!-- pytest-codeblocks:cont -->

```python
empty_selection = values[values > 100]

print(empty_selection)
print(empty_selection.shape)
print(empty_selection.size)
```

Ein leeres Ergebnis ist kein Fehler. Es ist einfach ein Array mit `size == 0`.

## Mehrere Bedingungen kombinieren

Für NumPy-Masken verwenden wir:

- `&` für elementweises **und**
- `|` für elementweises **oder**

Jede Teilbedingung muss dabei geklammert werden.

```python
import numpy as np

values = np.array([1, 2, 3, 4, 5, 6, 7, 8])

middle = values[(values >= 3) & (values <= 6)]
outside = values[(values < 3) | (values > 6)]

print(middle)
print(outside)
```

Für solche Array-Bedingungen verwenden wir **nicht** Python-`and` beziehungsweise `or`.

Also:

<!-- pytest-codeblocks:skip -->

```python
(values >= 3) & (values <= 6)
```

und nicht:

```text
values >= 3 and values <= 6
```

## Arrays verändern und unabhängig kopieren

NumPy-Arrays sind veränderbar:

```python
import numpy as np

data = np.array([10, 20, 30, 40])

data[0] = 99
data[data >= 30] = -1

print(data)
```

Wenn wir eine **unabhängige Kopie** brauchen, verwenden wir `.copy()`:

```python
import numpy as np

original = np.array(
    [[1, 2, 3],
     [4, 5, 6]]
)

copied = original.copy()

copied[0, 0] = 99
copied[copied >= 5] = -1

print("Original:")
print(original)

print("Kopie:")
print(copied)
```

Das Original bleibt unverändert.

Das ist wichtig, weil NumPy bei manchen Operationen bewusst Speicher zwischen Arrays teilen kann. Wenn wirklich eine unabhängige Version benötigt wird, macht `.copy()` diese Absicht eindeutig.

## Kennzahlen über alle Werte

NumPy bringt viele häufig benötigte Berechnungen direkt mit:

```python
import numpy as np

data = np.array(
    [[1, 2, 3],
     [4, 5, 6]]
)

print("min:", data.min())
print("max:", data.max())
print("sum:", data.sum())
print("mean:", data.mean())
```

Ohne `axis` werden dabei alle Werte des Arrays gemeinsam betrachtet.

Für das Beispiel:

```text
1 2 3
4 5 6
```

gilt:

- Minimum: `1`
- Maximum: `6`
- Summe: `21`
- Mittelwert: `3.5`

## Berechnungen entlang einer Achse

Bei 2D-Daten wollen wir häufig eine Kennzahl **pro Spalte** oder **pro Zeile** berechnen.

Wir markieren die Gruppen zunächst gedanklich:

```text
Spalten (axis=0):

  ↓  ↓  ↓
  1  2  3
  4  5  6

Zeilen (axis=1):

  1  2  3  →
  4  5  6  →
```

Mit `axis=0` wird über die Zeilen hinweg gerechnet. Übrig bleibt **ein Ergebnis pro Spalte**:

```python
import numpy as np

data = np.array(
    [[1, 2, 3],
     [4, 5, 6]]
)

print(data.sum(axis=0))
print(data.mean(axis=0))
```

Ergebnis:

```text
[5 7 9]
[2.5 3.5 4.5]
```

Mit `axis=1` wird über die Spalten hinweg gerechnet. Übrig bleibt **ein Ergebnis pro Zeile**:

<!-- pytest-codeblocks:cont -->

```python
print(data.sum(axis=1))
print(data.mean(axis=1))
```

Ergebnis:

```text
[ 6 15]
[2. 5.]
```

Die gleiche Idee funktioniert auch mit `min()` und `max()`:

<!-- pytest-codeblocks:cont -->

```python
print(data.min(axis=0))
print(data.max(axis=1))
```

Ein hilfreicher Merksatz ist:

> `axis` bezeichnet die Achse, **entlang der reduziert wird**.

Bei einem 2D-Array bedeutet deshalb:

- `axis=0` → ein Ergebnis pro **Spalte**
- `axis=1` → ein Ergebnis pro **Zeile**

## Zufallszahlen mit NumPy

NumPy kann auch direkt ganze Arrays aus Zufallszahlen erzeugen.

Für neuen Code wird dafür ein Zufallsgenerator angelegt:

```python
import numpy as np

rng = np.random.default_rng()

random_floats = rng.random((2, 3))
random_integers = rng.integers(1, 7, size=(2, 5))

print(random_floats)
print(random_integers)
```

`rng.random((2, 3))` erzeugt Zufallszahlen zwischen `0` und `1`.

`rng.integers(1, 7, ...)` erzeugt ganze Zahlen von `1` bis `6`: Wie bei `range()` und `arange()` ist die obere Grenze `7` **exklusiv**.

Ältere Beispiele verwenden häufig Funktionen wie `np.random.randint(...)`. Diese existieren weiterhin, gehören aber zur älteren Zufallszahlen-API. Für neuen Code verwenden wir hier `default_rng()` und `Generator.integers()`.

## Zwischenfazit

Für die nächsten Aufgaben sind vor allem diese Muster wichtig:

```python
import numpy as np

data = np.arange(1, 13).reshape(3, 4)

print(data.shape)
print(data[:, 1])
print(data[data >= 6])
print(data[(data >= 4) & (data <= 9)])
print(data.mean(axis=0))
```

Wenn ihr diese Ausdrücke lesen und die Form der Ergebnisse ungefähr vorhersagen könnt, habt ihr die wichtigsten Grundlagen für das Arbeiten mit tabellarischen NumPy-Daten.
