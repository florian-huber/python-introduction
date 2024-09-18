# Numerische Daten in Python mit NumPy 

`NumPy` ist die wohl wichtigste Python-Bibliothek wenn es um das Arbeiten mit (numerischen) Daten geht, nicht umsonst kommt der Name von **Num**erical **Py**thon. NumPy bringt nicht nur einen zentralen neuen Datentypen mit, das **Numpy-Array**, sondern auch eine Vielzahl von Funktionen und Methoden um z.B. mit Vektoren, Matrizen, und Tensoren zu arbeiten.

In der Regel, vereinfacht NumPy dabei nicht nur den Programmieraufwand, sondern es ist auch stark auf Rechengeschwindigkeit optimiert (die Kernmethoden laufen über kompilierte C-Bibliotheken). 

Aber vielleicht beginnen wir besser mit einem Beispiel.

Stellen wir uns vor, wir haben Umfragedaten erhoben. Bei der Umfrage mussten alle Personen angeben wie sehr bestimmte Aussagen für sie zutreffen, von 1 (gar nicht) bis 5 (völlig). Wenn wir die Daten in Python importieren bekommen wir dann z.B. die Ergebnisse jeder Person in Form einer Liste. Nun sollen die Antworten zweier Personen verglichen werden. Zum Beispiel sollte dort jeweils die Differenz der Werte an gleicher Stelle berechnet werden. Mit den bisherigen Mitteln könnten wir das etwa so programmieren:

```python
umfrage_person1 = [2, 3, 5, 4, 5, 2, 1, 3, 4, 4, 3]
umfrage_person2 = [4, 4, 2, 2, 4, 4, 3, 1, 5, 4, 2]

umfrage_diffs = []
for i in range(len(umfrage_person1)):
    umfrage_diffs.append(umfrage_person1[i] - umfrage_person2[i])
    
print(umfrage_diffs)

# Jetzt will ich aber nur die Beträge --> abs()
print([abs(x) for x in umfrage_diffs])
```

Mit NumPy wäre das Ganze deutlich einfacher möglich:

```python
import numpy as np

umfrage_person1 = np.array([2, 3, 5, 4, 5, 2, 1, 3, 4, 4, 3])
umfrage_person2 = np.array([4, 4, 2, 2, 4, 4, 3, 1, 5, 4, 2])

print(umfrage_person1 - umfrage_person2)
print(abs(umfrage_person1 - umfrage_person2))
```

Hier haben wir gleich den zentralen, neuen Datentypen benutzt: Numpy Arrays!

<!-- pytest-codeblocks:cont -->

```python
print(type(umfrage_person1))  # => <class 'numpy.ndarray'>
```



Numpy Arrays können also einfach aus Python Listen erstellt werden.

```python
import numpy as np

a = np.array([2, 5.9, 7.7, 100.0])
b = np.array([[2, 4, 7],
              [3, 5, 11]])

print(a)
print(b)
```

Numpy Arrays unterscheiden sich in vieler Hinsicht von den Listen:

- Alle Elemente haben den selben Datentyp (z.B. alles float oder int). Darüber hinaus auch eine Vielzahl von weiteren numerischen Datentypen (https://numpy.org/doc/stable/user/basics.types.html)
- Bei Tabellen/Matrizen/Tensoren müssen alle Einträge angegeben werden, d.h. es kann keine Tabelle angelegt werden in der die verschiedenen Zeilen unterschiedliche Anzahlen an Elementen enthalten. Bei einer verschachtelten Liste würde das gehen.
- Bei Zahlen passt Python den Speicherbedarf dynamisch an. In Numpy Arrays wird allen Zahlen der gleiche Speicherplatz zur Verfügung gestellt also z.B. alle Einträge als `int64` (64 bit) auch wenn eventuell oft auch `int8` (8 bit) ausreichen würde.

### Informationen zum Numpy Array

Wie gerade erwähnt, gibt es viele Möglichkeiten numerische Werte in einem Numpy Array zu speichern. Um den entsprechenden Datentypen abzufragen nutzen wir `.dtype`. Darüber hinaus kann auch die Form (shape) abgefragt werden mit `.shape` und die Gesamtanzahl der Elemente mit  `.size`. Die Dimension des Arrays bekommen wir mit `.ndim`.

```python
import numpy as np

b = np.array([[2, 4, 7],
              [3, 5, 11]])

print("Matrix shape:", b.shape)
print("Total number of elements:", b.size)
print("Matrix dimensions:", b.ndim)
print("Data type:", b.dtype)
```



### Slicing und Indexing

Bei Listen (und anderen Datentypen) haben wir bereits mit dem sogenannten Slicing gearbeitet. Damit können z.B. bestimmte Teile einer Liste wiedergegeben werden. Das gleiche geht nun auch für Numpy Arrays (nur besser!). Erstmal ein direkter Vergleich:

```python
import numpy as np

matrix_list = [[1, 2, 3],
         	   [4, 5, 6],
               [7, 8, 9]]

print(matrix_list[1])
print(matrix_list[1][2])

matrix = np.array(
    [[1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]])

print(matrix[1])
print(matrix[1][2])
```

> Mini-Quiz: Welche Zahl bekommen wir für `matrix[1][2]`?

Soweit, so gleich. Aber Numpy Arrays können auch mit einer Multi-Index-Schreibweise genutzt werden (was auch die gängige Praxis ist). 

<!-- pytest-codeblocks:cont -->

```python
print(matrix[1, 2])

# Und damit gehen auch andere slicing optionen:
print("First row:", matrix[0, :])
print("Last column:", matrix[:, -1])
print("Lower right corner:", matrix[1:, 1:])
```

Die Multi-Index-Schreibweise macht es bei mehrdimensionalen Objekten deutlich einfacher, bestimmte Teile davon zu adressieren. 

Numpy Arrays bieten aber noch deutlich weitergehende Möglichkeiten um bestimmte Elemente aufzurufen etwa über Index-Listen oder Index-Arrays:

```python
import numpy as np

matrix = np.array(
    [[1, 2, 3],
     [4, 5, 6],
     [7, 8, 9]])

indices = [2, 0, 2, 0]
print(matrix[indices])

# oder als Numpy Array
indices = np.array([2, 0, 2, 0])
print(matrix[indices])
```

Es gehen sogar auch Boolean als Index:

<!-- pytest-codeblocks:cont -->

```python
mask = [True, False, True]
print(matrix[mask])
```

Aber was bringt das jetzt, dass ich mit Boolean bestimmte Elemente adressieren kann?

Das Ganze macht v.a. dann Sinn wenn es mit Bedingungen/Abfragen verbunden wird! Werte in Numpy Arrays können nämlich einfach über Bedingungen abgefragt werden:

```python
import numpy as np

matrix = np.array(
    [[1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]])

print(matrix >= 5)
```

Und dieses Array aus True/False Werten kann dann wiederum als Index benutzt werden:

<!-- pytest-codeblocks:cont -->

```python
print(matrix[matrix >= 5])

# Damit können Elemente auch angepasst werden:
matrix[matrix >= 5] = 5
print(matrix)
```

Es lassen sich auch kompliziertere logische Abfragen bauen indem wir `&` als element-wise `and` oder `|` als element-wise `or` benutzen:

```python
import numpy as np

matrix = np.array(
    [[1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]])

print(matrix[(matrix >= 2) & (matrix < 7)])
print(matrix[(matrix < 3) | (matrix > 7)])
```

### Operationen mit Numpy Arrays (rechnen...)

In dem Eingangsbeispiel zum Kennenlernprojekt haben wir schon gesehen, dass sich mit Numpy Arrays ziemlich komfortabel rechnen lässt. Numpy Array "verstehen" nämlich verschiedene Rechenoperationen die häufig nützlich sind. So gehen z.B. die typischen Rechenoperationen (+, -, /, *) sowohl zwischen einem Array und einem einzelnen Wert, als auch zwischen zwei Arrays!

```python
import numpy as np

numbers = np.array([4, 5, 2, 3, 1, 5, 5, 2])
print(numbers - 5)

other_numbers = np.array([2, 5, 1, 1, 4, 3, 4, 3])
print(numbers - other_numbers)
```

Und genauso mit anderen Rechenarten, z.B:

<!-- pytest-codeblocks:cont -->

```python
numbers = np.array([4, 5, 2, 3, 1, 5, 5, 2])
print(numbers * 5)

other_numbers = np.array([2, 5, 1, 1, 4, 3, 4, 3])
print(numbers * other_numbers)
```

### Weitere Operationen mit Arrays

NumPy bietet viele praktische Methoden um numerische/statistische Operationen anzuwenden. Diese können als Methode aufgerufen werden(`matrix.sum()`) oder als `np.sum(matrix)` (völlig gleich).

```python
import numpy as np

matrix = np.array(
    [[1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]])

print(matrix.sum())
print(np.sum(matrix))
```

`.sum()` bildet hierbei die Summe aller Elemente vom `matrix`-Array. Darüber hinaus können aber auch andere Summen gebildet werden:

<!-- pytest-codeblocks:cont -->

```python
print(matrix.sum())
print(matrix.sum(axis=0))
print(matrix.sum(axis=1))
```

> Mini-Quiz: Was machen diese Summen-Methoden?

Auf die gleiche Art lassen sich auch die Methoden `.max()` und `.min()` nutzen um die maximal- bzw. minimal-Werte zu finden. 

> Was für Ergebnisse erwarten wir hier?

```python
import numpy as np

matrix = np.array(
    [[1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]])

print(matrix.max())
print(matrix.max(axis=0))
print(matrix.max(axis=1))

print(matrix.min())
print(matrix.min(axis=0))
print(matrix.min(axis=1))
```



### Arrays erstellen

Bisher haben wir gesehen, wie man Numpy Arrays aus Listen erstellt. Es gibt aber noch andere Möglichkeiten um Numpy Arrays zu generieren:

+ `np.zeros()` und `np.ones()`
  Damit können beliebig geformte Arrays aus lauter Nullen (zeros) oder Einsen (ones) erstellt werden:

  ```python
  import numpy as np
  
  data = np.zeros((3, 4))  # hier: 2 Dimensionen, aber es können auch mehr werden!
  print(data)
  print(data.shape)
  
  data = np.ones((3, 4))
  print(data)
  print(data.shape)
  ```

+ `np.random.random()`
  Damit können beliebig geformte Arrays aus lauter Zufallszahlen erstellt werden:
  <!-- pytest-codeblocks:cont -->

  ```python
  data = np.random.random((3, 4))
  print(data)
  print(data.max())
  ```

  Zu dem Untermodul `.random` gehören noch viele weitere Zufallsgeneratoren die wir nutzen können. U.a. auch `randint` um zufällige Integer Werte in einem vorgegebenen Bereich auszugeben.
  
+ `np.arange()`
  Das ist das Pendant zur `range()` Funktion in Python. Es benötigt einen Startwert, Endwert und eine Schrittgröße.

  ```python
  import numpy as np
  
  data = np.arange(-10, 10, 1)  # es gehen natürlich auch floats!
  print(data)
  ```

  

+ `np.linspace()`
  Ähnlich wie `np.arange()` , doch hier wird nicht eine Schrittgröße sondern die Anzahl der Schritte festgelegt!

  ```python
  import numpy as np
  
  data = np.linspace(-10, 10, num=10)
  print(data)
  ```

### Numpy Arrays verändern

Wir haben oben schon gesehen, dass Numpy Arrays veränderbar sind. Wie bei Listen auch, können einzelne Werte verändert werden, aber auch ganze Teile (mit slicing):

<!-- pytest-codeblocks:cont -->

```python
data = np.array([1, 2, 3, 4, 5, 6, 7, 8])
data[-1] = 12
data[:3] = -1
print(data)
```

Es können aber auch verschiedene Arrays zusammengefügt werden mit `np.concatenate()`:

```python
import numpy as np

a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])

arr = np.concatenate((a, b))
print(arr)
```

Bei mehr-dimensionalen Arrays muss noch angegeben werden in welche "Richtung"  (axis) die Arrays zusammengefügt werden sollen:

<!-- pytest-codeblocks:cont -->

```python
a = np.array([[1, 2],
              [3, 4]])
b = np.array([[5, 6],
              [7, 8]])

arr = np.concatenate((a, b), axis=0)
print(arr)

arr = np.concatenate((a, b), axis=1)
print(arr)
```

Darüber hinaus kann auch die Form (shape) eines Arrays verändert werden mit `.reshape()` bzw mit `.flatten()`.

```python
import numpy as np

data = np.array([1, 2, 3, 4, 5, 6])

data_reshaped = data.reshape(3, 2)
print(data_reshaped)

# und zurück
data_flattened = data_reshaped.flatten()
print(data_flattened)

# reshape variante 2
print(data.reshape(2, 3))
```



