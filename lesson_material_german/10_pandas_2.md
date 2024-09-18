# Pandas - Teil 2

Im ersten Teil haben wir gesehen, wie wir eine `.csv`-Datei einlesen und grundlegende Eigenschaften erkunden. Hier werden wir jetzt noch einige Schritte weitergehen im Umgang mit (komplexen) Datensets.

## Data cleaning

Unter data cleaning versteht man das Umstellen und Bearbeiten von Daten mit dem Ziel, diese für die folgende Analyse besser nutzbar zu machen. Einen Schritt in diese Richtung haben wir letztes Mal bereits unternommen, indem wir die Anzahl der Spalten auf die wichtigsten reduziert haben.

```python
import pandas as pd
import numpy as np
import os

root = os.getcwd()
filename = os.path.join(root, "my_data_folder", 'pokemon.csv')

data = pd.read_csv(filename, delimiter=",")

cols = ['name', 'type1', 'type2', 'speed', 'abilities','attack',
       'defense', 'height_m', 'hp', 'weight_kg',
       'generation', 'is_legendary']
data_selected = data[cols]
```

Mit `data_selected.info()` können wir sehen, das einige Spalten nicht die benötigten 801 Einträge enthalten.

Wenn wir uns die Tabelle anschauen (z.B. mit `data_selected.head()`) , sehen wir, dass einige Einträge `NaN` heißen. Dies steht für Not-a-Number und ist das Numpy Format für fehlende/unbekannte Einträge. 

Pandas bietet eine Reihe von Möglichkeiten mit solchen fehlenden Werten  umzugehen. NaN's gibt es auch in Numpy, nur haben wir sie uns dort nicht explizit  angeschaut. In Numpy kann solch ein Eintrag erstellt werden als `np.nan`.

In Pandas können diese über `.isnull()` oder `isna()` abgefragt werden, wobei das `.any()` angibt das der Wert True ausgegeben werden soll wenn mindestens ein Element ein NaN ist. Beide Methoden machen hier das Gleiche auch wenn "isnull" vielleicht verwirrend ist, denn es gibt nicht wieder ob ein Wert Null ist, sondern nur ob ein Wert nicht vorhanden ist (NaN --> True).

```python
print(data_selected.isna().any())

print(data_selected.isnull().any())
```

Wir können nun alle Spalten anzeigen in denen einzelne Werte fehlen:

```python
data_selected[data.columns[data.isna().any()]]
```

### Und jetzt? Was tun mit NaNs?

Je nachdem was wir mit den Daten vorhaben, gibt es verschiedene Strategien mit den fehlenden Einträgen umzugehen. In vielen Fällen können wir sie einfach ignorieren. Wenn wir Mittelwerte (`.mean()`) oder Ähnliches berechnen, werde diese Werte automatisch nicht mitgezählt.

In anderen Fällen wollen wir die Werte vielleicht ersetzten, z.B. durch "0". Aber Vorsicht! Danach werden diese Werte nämlich doch mitgezählt wenn Summen oder Mittelwerte berechnet werden.

#### NaN ersetzen

Das Ersetzen geht bei Pandas mit `.fillna()`:

```python
data_selected.fillna(0).head()
```

Wir können aber auch Strings oder andere Formate benutzen:

```python
data_selected.fillna("unknown").head()
```

Das kann sinnvoll sein, wenn wir die "unknown" explizit mitzählen möchten:

```python
data_selected.fillna("unknown")["type2"].value_counts()
```

### NaN entfernen

Falls NaN nicht einfach ersetzt werden können und für die weiteren Schritte nicht vorkommen sollen, dann müssen entweder die entsprechenden Spalten oder Zeilen aus der Tabelle entfernt werden. Das geht in Pandas mit `.dropna()`.

```python
data_selected.dropna().head()
```

Oder für die Entfernung aller Spalten mit NaNs:

```python
data_selected.dropna(axis=1).head()
```

### Wichtig:

In Python gibt es zwei Möglichkeiten für Methoden die "ihr" Objekt verändern wollen:

1) Objekt bleibt unverändert und veränderte Kopie wird über `return` ausgegeben.
2) Objekt selbst wird verändert.

Bei Pandas findet in der Regel (1) statt, siehe auch https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-view-versus-copy.

Das bedeutet, wenn wir ein verändertes DataFrame erhalten wollen müssen wir dies mit `=` erstellen:

```python
data_na_removed = data_selected.dropna()
data_na_removed.head()
```

## Digging a bit deeper...

Gehen wir mit der Analyse der Daten einen Schritt weiter. Was wenn wir mehr über einzelne Gruppen wissen möchten?

Über Masken können wir ganz gezielt einzelne Fragen adressieren:

```python
mask = data_selected["type1"] == "bug"
data_selected[mask].head()
```

Über solche Masken können wir auch Eigenschaften über einzelne Gruppen abfragen:

```python
data_selected[mask]["weight_kg"].mean()
```

Das geht im Prinzip ganz gut und macht was es soll. Doch Pandas hat dafür (natürlich) auch eine Funktion die das noch einfacher macht: `groupby()`.

## Groupby

Pandas `grouby` (siehe [Pandas documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)).

Mit grouby können bei Pandas alle Einträge mit bestimmten Kategorien zusammengefasst werden. Damit lassen sich leicht und schnell viele Eigenschaften vergleichen.

```python
data_selected.groupby("type1").mean()
```

Hier sind die Einträge nach dem "type1" alphabetisch sortiert. Um die Sortierung zu ändern fügen wir einfach ein `sort_values()`hinzu um nach der gewünschten Spalte zu sortieren:

```python
data_selected.groupby("type1").mean().sort_values("speed")
```

Groupby kann auch genutzt werden um die Anzahl der Elemente in den jeweiligen Gruppen zu zählen:

```python
data_selected.groupby("type1").count()
```

Es ist sogar möglich, mehrere groupby Aktionen zu kombinieren und damit Gruppen und Untergruppen zu erstellen:

```python
data_selected.groupby(["type1", "is_legendary"]).mean()
```

Damit können wir einsehen, welche mittleren Werte ein Pokemon von einem bestimmtem Typ hat, je nachdem ob er "legendary" ist oder nicht.



## Plotting

Noch einige kurze Plotting-Beispiele zum Schluss.

Da wir eben `groupby` benutzt haben, können wir dies auch verwenden um einen Plot zu erzeugen. "stacked=True" bedeutet hierbei übrigens, dass die verschiedenen Balken aneinander gehängt werden sollen.

```python
cols = ["attack", "defense", "hp", "speed", "weight_kg"]
data_selected.groupby("type1").mean()[cols].plot.barh(stacked=True)
```

Es gibt schönere Plots, aber zumindest sehen wir so sehr schnell, dass vor allem der Eintrag "weight_kg" sehr stark von der jeweiligen type1-Gruppe abhängt! Also gleich noch einen Plot dafür hinterher:

```python
data_selected.groupby("type1").mean()["weight_kg"].plot.barh()
```

#### Intermezzo: data visualization

Warum ist der Plot hier oben ziemlich mies?

--> Die Reihenfolge macht es **sehr** unübersichtlich! Stellen Sie sich vor ich frage, welcher Typ auf der Position 3, 4, oder 5 steht...

--> Die Axenbeschriftung bei der x-Axe fehlt.

Allgemein: Pandas plottet mit Hilfe von [`matplotlib`](https://matplotlib.org/). Für Aufwendigere Grafiken und Anpassungen werden die Plots darum auch  mit matplotlib programmiert. Aber Pandas gibt eben einen schnellen  Zugang um verschiedene Plots zu erkunden.

Einfache Anpassungen lassen sich auch durchaus gut mit Pandas machen, bzw. können auch matplotlib Befehle mit Pandas kombiniert werden.

Darum hier ein weiterer Versuch:

```python
ax = data_selected.groupby("type1").mean()["weight_kg"].sort_values().plot.barh()
ax.set_xlabel("weight [kg]")
```

Bei Pandas werden solche Aufrufe schnell sehr lang und kompliziert, da  immer mehr und mehr Funktionen/Methoden hintereinander gehangen werden  können. Um es etwas übersichtlicher zu machen kann auch mit  Zwischenschritten gearbeitet werden!

```python
data_groupby_type = data_selected.groupby("type1")
data_mean_weight = data_groupby_type.mean()["weight_kg"]
ax = data_mean_weight.sort_values().plot.barh()
ax.set_xlabel("weight [kg]")
```



### Einige wenige weitere Beispiele im Jupyter Notebook der Vorlesung

Der gesamte Code und einige weitere Plot Beispiele finden Sie im Jupyter Notebook `vorlesung_10_intro_pandas_2.ipynb` auf Moodle.



## Pandas ist riesig!

Zum Schluss noch der Hinweis, das wir in 2-3, ja auch nicht in 4-5 Vorlesung den Umfang von Pandas abdecken können. Es geht also v.a. darum, dass Sie sich mit dem Umgang mit Pandas und DataFrames vertraut machen und die einfachen Operationen damit ausführen können (z.B. slicing, Sortieren). Für alles weitere können Sie jederzeit in Foren oder auf der Pandas Dokumentation nachschauen, ob es für ihre spezielle Frage nicht schon eine passende Pandas-Methode gibt:

https://pandas.pydata.org/pandas-docs/stable/reference/frame.html