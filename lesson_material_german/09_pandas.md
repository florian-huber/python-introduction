**Jupyter Notebook**

Bisher haben wir unseren Python Code entweder in einer Konsole geschrieben (eigentlich nur test-weise), oder eben hauptsächlich in einer Entwicklungsumgebung wie Spyder. Letzteres ist und bleibt auch der bevorzugte weg um umfangreicheren Code zu entwickeln.

Allerdings gibt es noch eine alternative Umgebung in der sehr gut mit Python gearbeitet werden kann und die sich besonders für Data Science Aufgaben, bzw. erste Erkundungen von Datensets eignet: **Jupyter Notebook**. Die ganzen folgenden Code-Beispiele werden darum auch in solchen Notebooks dargestellt.

Jupyter Notebook ist Teil der Anaconda-Installation und erzeugt Notebook-Dateien die mit `.ipynb` enden. Jupyter Notebook kann über das Startmenü (unter Anaconda) gestartet werden, oder über den Befehl `jupyter notebook` aus dem Terminal.

In der Vorlesung wird die Verwendung von Jupyter notebooks vorgeführt und wir werden uns in der nächsten Übung auch damit beschäftigen. Ansonsten gibt es im Internet viel Material zur Benutzung der notebooks, z.B. hier: https://realpython.com/jupyter-notebook-introduction/.



# Pandas!

Numpy ist **das** Python-Tool wenn es um den Umgang mit großen Zahlenmengen geht. Aber für viele Data Science-artige Aufgaben fehlen bestimmte Dinge. Und genau darum gibt es **Pandas** (das intern u.a. Numpy Arrays benutzt):

- Heterogene Datentypen innerhalb einer Tabelle
- Labels
- Mehr Möglichkeiten zum Umgang mit fehlenden Werten
- Mehr Datentypen

Pandas (der Name steht für **Pan**el **Da**ta) wird üblicherweise wie folgt importiert:

```python
import pandas as pd
```

In Numpy haben wir uns v.a. mit den Numpy Arrays beschäftigt, der zentrale Datentyp in Numpy.

In Pandas gibt es zwei Datentypen mit denen wir arbeiten werden: **DataFrame** und **Series**. Wir starten mit ersterem. 

```python
import pandas as pd
import numpy as np

my_dict = {"city": ["Essen", "Bochum", "Dortmund"],
           "inhabitants": [591032, 364454, 587696],
           "xyz": np.random.random(3)}

df = pd.DataFrame(my_dict, index=["a", "b", "c"])
```

Hier haben wir ein DataFrame mit Hilfe eines Dictionaries erstellt. Dabei werden dann die *keys* zu den Spaltennamen und die Zeilen bekommen über `index` die gewünschten Namen zugewiesen. Alternativ kann `index` auch weggelassen werden, dann nummeriert Pandas de Zeilen einfach mit 0, 1, 2...

Aber wie schauen wir uns das ganze jetzt eigentlich an?

In diesem Fall hier haben wir ja noch eine relativ kleine Tabelle und können sie uns einfach mit `print(df)` anschauen. Im Jupyter notebook geht es noch einfacher und wir geben einfach nur `df` ein.

Pandas DataFrames können auch auf viele andere Arten erstellt werden, z.B. aus Listen:

```python
import pandas as pd
import numpy as np

my_list = [["Essen", 591032, np.random.random(1)[0]],
           ["Bochum", 364454, np.random.random(1)[0]],
           ["Dortmund", 587696, np.random.random(1)[0]]]

df = pd.DataFrame(my_list,
                  columns=["city", "inhabitants", "xyz"],
                  index=["a", "b", "c"])
```



## Slicing und Indexing

Slicing und Indexing, also das gezielte "zerschneiden" (slicing) und Adressieren (indexing) von Tabellen um nur bestimmte Daten wiederzugeben ist eines der Zentralen Elemente bei der Arbeit mit größeren Datenmengen. Bei Numpy haben wir schon gesehen, wie mächtig, aber auch wie schwierig das sein kann. In Pandas gibt es sogar oft noch mehr Optionen und es kann schnell etwas unübersichtlich werden. Allerdings: Gute Slicing/Indexing-Skills in Pandas werden ziemlich schnell zu echten Superkräften wenn es um Data Science geht ;)

OK, aber aller Anfang... 

Wir haben gerade gesehen, dass ein Dictionary in ein DataFrame umgesetzt werden kann. Und die Inhalte können auch auf eine Ähnliche Art abgerufen werden, nämlich über die Spaltennamen (entweder einzeln oder als Liste):

<!-- pytest-codeblocks:cont -->

```python
df["inhabitants"]

cols = ["city", "inhabitants"]
df[cols]
```

Um bestimmte Zeilen auszuwählen ist es möglich `.loc` zu nutzen, also `.loc["a"]` für eine Zeile oder eine Liste oder Range für mehrere Zeilen

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"city": ["Essen", "Bochum", "Dortmund"],
                   "inhabitants": [591032, 364454, 587696],
                   "xyz": np.random.random(3)},
                  index=["a", "b", "c"])
                  
df.loc["a"]

# oder über liste
rows = ["a", "c"]
df.loc[rows]

# oder über range mit :
df.loc["a":"b"]

# Das kann auch mit Spalten kombiniert werden
df.loc["a", ["city", "inhabitants"]]
```

Alternativ gibt es noch einen Weg über die Indices und zwar mit `.iloc` und das sieht dann plötzlich wieder aus wie bei Numpy:

<!-- pytest-codeblocks:cont -->

```python
df.iloc[0:2]

# oder auch Zeilen und Spalten
df.iloc[0:2, 1:]
```

Zusammenfassung:

- `[ ]` um Spalten zu wählen
- `.loc[row_labels, column_labels]`  um über die Labels (Zeilen und Spaltennamen) Elemente auszuwählen
- `.iloc[row_positions, column_positions]`  um über die Indices Elemente auszuwählen

### Elemente hinzufügen/entfernen

Einem DataFrame können weitere Spalten genau wie bei Dictionaries hinzugefügt werden:

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"city": ["Essen", "Bochum", "Dortmund"],
                   "inhabitants": [591032, 364454, 587696],
                   "xyz": np.random.random(3)},
                  index=["a", "b", "c"])

df["height"] = [116, 100, 86]
df
```

Der umgekehrte Schritt wäre, eine Spalte komplett zu entfernen, das geht mit `del df["xyz"]`.



### Now: go Data Science

*Den folgenden Teil kann man wahrscheinlich besser über das entsprechende Jupyter Notebook nachvollziehen (bzw. auch gleich damit arbeiten!).*

Bisher haben wir uns nur einfache Beispiele angeschaut um die Methoden von Pandas ein wenig kennenzulernen. Interessant wird das Ganze aber eigentlich erst wenn wir anfangen mit größeren Datensets zu spielen. Wann ein Datenset als "groß" gilt ist natürlich relativ, hier meine ich damit v.a. Daten die zumindest so groß sind, dass wir sie nicht mit einem kurzem Blick auf eine Tabelle vollständig verstehen können.

Für diesen Zweck fangen wir hier an mit dem "Pokemon Dataset" zu arbeiten das man auf "Kaggle" herunterladen kann: https://www.kaggle.com/rounakbanik/pokemon

### Daten importieren

Das Laden von Daten aus Textdateien geht in der Regel recht einfach mit Pandas, da Pandas entsprechende Funktionen dafür bereitstellt. U.a. zum importieren von CSV oder Excel-Dateien.


```python
import os

root = os.getcwd()
filename = os.path.join(root, "exercises", 'pokemon.csv')  # je nach eigenem Pfad anpassen!

data = pd.read_csv(filename, delimiter=",")
# hier geht auch: data = pd.read_csv(filename) da delimiter="," der "default"-Parameter ist

data.head() # oder: data.tail()
```

Die Tabelle ist hier erstmal zu groß, darum können wir uns erst die Spaltennamen anschauen:


```python
data.columns
```

#### Neues, kleineres DataFrame erzeugen

Das sind ziemlich viele Spalten! Wir nehmen darum erstmal nur ein paar davon für die nächsten Schritte


```python
cols = ['name', 'type1', 'type2', 'speed', 'abilities','attack',
       'defense', 'height_m', 'hp', 'weight_kg',
       'generation', 'is_legendary']
data_selected = data[cols]
data_selected.head()
```

Einen ersten Eindruck der Daten (zumindest der numerischen Einträge), können wir uns verschaffen über:


```python
data_selected.describe()
```

Danach kommen wir zum eigentlichen Kern der Pandas Analyse. Durch das Kombinieren von Slicing/Indexing und weiteren Pandas-Methoden können verschiedene Aspekte der Daten schnell und gut erkundet werden.

Z.B um zu schauen welche Typen von Pokemon es gibt


```python
data_selected["type1"].unique()
```

Und wie viele es von jedem Typ gibt:


```python
data_selected["type1"].value_counts()
```

### Wie bekomme ich nun die Werte

Pandas gibt in den meisten Fällen entweder ein **DataFrame** (Tabelle) zurück, oder eine **Series** (eine einzelne Spalte mit Zeilennamen). In manchen Fällen wollen wir aber nur die eigentlichen Werte aus der Spalte (oder aus der Tabelle) haben. Dafür gibt es in Pandas die Möglichkeit ein Numpy Array auszugeben mit `.values`.


```python
values = data_selected["type1"].value_counts().values
print(values)
print(type(values))
```

    [114 105  78  72  53  52  45  39  32  32  29  28  27  27  24  23  18   3]
    <class 'numpy.ndarray'>

## Sortieren

Anders als by Numpy und Listen gibt es bei Pandas keine Methode `.sort()`, sondern zwei verschiedene Methoden:  

- `.sort_index()` - Sortiert nach dem Index
- `.sort_values()` - Sortiert nach Spaltenwerten

Anders als bei Listen gibt es kein Parameter "reverse" sondern dafür `ascending`, das zur Umkehrung der Sortierung auf `False` gesetzte werden kann.


```python
data_selected.sort_values(["speed", "name"])

# or: data_selected.sort_values(["speed", "name"], ascending=False)
```



