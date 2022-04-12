# NumPy 

`NumPy` ist die wohl wichtigste Python-Bibliothek wenn es um das Arbeiten mit (numerischen) Daten geht, nicht umsonst kommt der Name von **Num**erical **Py**thon. NumPy bringt nicht nur einen zentralen neuen Datentypen mit, das Numpy-Array, sondern auch eine Vielzahl von Funktionen und Methoden um z.B. mit Vektoren, Matrizen, und Tensoren zu arbeiten.

In der Regel, vereinfacht NumPy dabei nicht nur den Programmieraufwand, sondern es ist auch stark auf Rechengeschwindigkeit optimiert (die Kernmethoden laufen über kompilierte C-Bibliotheken). 

Aber vielleicht beginnen wir besser mit einem Beispiel.

Für das erste Programmierprojekt (KennenlernProjekt) kamen alle Gruppen zu einer Stelle an der die Zahlen zweier Listen verglichen werden sollten. Zum Beispiel sollte dort jeweils die Differenz der Werte an gleicher Stelle berechnet werden. Das konnte etwa so gelöst werden:

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

- Alle Elemente haben den selben Datentyp (z.B. alles floats). Darüber hinaus auch eine Vielzahl von numerischen Datentypen (https://numpy.org/doc/stable/user/basics.types.html)
- Bei Tabellen/Matrizen/Tensoren müssen alle Einträge angegeben werden, d.h. es kann keine Tabelle angelegt werden in der die verschiedenen Zeilen unterschiedliche Anzahlen an Elementen enthalten.
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





### Berechnungen mit Numpy

Numpy Arrays sind in Python das Standard-Format um mit großen (numerischen) Datenmengen zu arbeiten. Bisher haben wir gesehen, was es alles für Grundfunktionen/methoden mit Numpy Arrays gibt. Jetzt aber noch ein paar Beispiele die illustrieren sollen, wie und warum diese Arrays so gerne in der Praxis eingesetzt werden.

Wir werden dafür eine weitere Bibliothek nutzen: `matplotlib`, das ist in Python die am häufigsten genutzte Bibliothek um Daten zu plotten.

**(1) Werte-Berechnungen mit mathematischen Funktionen.**
Numpy bringt einige mathematische Funktionen mit, z.B. `np.sin()`(Sinus), `np.cos()` (Cosinus), `np.exp()`(Exponentialfunktion) oder `np.log()`(Logarithmus). Sehr, sehr viele weiter Optionen finden sich z.B. in der Bibliothek SciPy (https://scipy.org/).

```python
import numpy as np
from matplotlib import pyplot as plt

x = np.linspace(-10, 10, 500)
y = np.sin(x)
plt.plot(x, y, "darkblue")
```

Hier werden 500 Datenpunkte erzeugt mit x-Werten die gleichmäßig zwischen -10 und 10 verteilt sind. Von allen wird der Sinus berechnet und anschließend graphisch dargestellt.

**(2) Operationen auf vielen Datenpunkten gleichzeitig + Zufallszahlen.**
Numpy bietet enorme Möglichkeiten für die Nutzung von Zufallszahlen. Wir hatten vorher auch schon die Bibliothek `random` kennengelernt, doch damit ließ sich immer nur eine Zufallszahl nach der anderen erzeugen. Mit Numpy können (für unsere Zwecke) beliebig große Arrays aus Zufallszahlen erzeugt werden. Darüber hinaus bietet Numpy sehr verschiedene Arten von Zufallsverteilungen! 

```python
import numpy as np
from matplotlib import pyplot as plt

xy = np.random.random((2, 100))
sizes = 100 * np.random.random(100)
plt.scatter(xy[0, :], xy[1, :], s=sizes)
```

Eine andere Verteilung bekommt man mit `np.random.randn()`(Normalverteilung!):

```python
import numpy as np
from matplotlib import pyplot as plt

xy = np.random.randn(2, 1000)
plt.scatter(xy[0, :], xy[1, :], alpha=0.3)
```

**(3) Statistik und Simulationen**
NumPy erlaubt es große Mengen an Zufallszahlen zu generieren und damit zu arbeiten. Das hat in der Praxis viele verschiedene Verwendungen, wie z.B. für die Modellierung von stochastischen Prozessen oder für Computer-Simulationen.

```python
import numpy as np

random_numbers = np.random.random((1000, 1000))
print(f"Mean: {random_numbers.mean()}")
print(f"Numbers > 0.5: {np.sum(random_numbers > 0.5)}")
print(f"Numbers < 0.5: {np.sum(random_numbers < 0.5)}")
```

**(4) Speed!**
Numpy ist zum Teil in Sprachen wie C implementiert und für viele Rechenoperationen deutlich schneller als das "normale" Python. Ein Beispiel:

<!-- pytest-codeblocks:cont -->

```python
import random
import time

print("Pure Python ......................")
generated_numbers = int(1e7)
tstart = time.time()
numbers = []
for _ in range(generated_numbers):
    numbers.append(random.random())
print(sum(numbers))
print(f"Calculation took {time.time() - tstart}s")

print("\nNumPy ......................")
tstart = time.time()
numbers = np.random.random(generated_numbers)
print(np.sum(numbers))
print(f"Calculation took {time.time() - tstart}s")
```

