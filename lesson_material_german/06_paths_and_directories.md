# Navigieren durch Ordner und Pfade

Programme arbeiten häufig mit Dateien, die nicht direkt neben dem Python-Skript liegen. Deshalb müssen wir wissen, **in welchem Ordner Python gerade arbeitet**, wie wir Pfade zusammensetzen und wie wir Dateien in Ordnern finden können.

In diesem Kapitel verwenden wir dafür das Modul `os`.

```python
import os
```

## Der aktuelle Arbeitsordner

Python hat immer einen aktuellen **Arbeitsordner** (*current working directory*). Relative Pfade werden von dort aus interpretiert.

Mit `os.getcwd()` können wir ihn anzeigen:

```python
import os

current_dir = os.getcwd()
print(current_dir)
```

Das Ergebnis ist ein **absoluter Pfad**, also ein vollständiger Pfad auf dem Computer.

Ein relativer Pfad beschreibt dagegen einen Ort **ausgehend vom aktuellen Arbeitsordner**.

Zum Beispiel:

```text
data/measurements.csv
```

ist relativ.

Ein absoluter Pfad könnte zum Beispiel so aussehen:

```text
/home/student/project/data/measurements.csv
```

oder unter Windows:

```text
C:\Users\student\project\data\measurements.csv
```

Absolute Pfade hängen also vom konkreten Computer ab. Relative Pfade sind für Projekte häufig praktischer.

## Pfade mit `os.path.join()` zusammensetzen

Pfade sollten nicht einfach mit `"/"` oder `"\\"` zusammengesetzt werden. Stattdessen verwenden wir `os.path.join()`:

```python
import os

folder = "data"
filename = "measurements.csv"

path = os.path.join(folder, filename)

print(path)
```

Je nach Betriebssystem verwendet Python dabei automatisch den passenden Pfadtrenner.

Auch mehrere Teile können verbunden werden:

```python
import os

path = os.path.join("data", "2026", "measurements.csv")
print(path)
```

## Absolute und relative Pfade darstellen

Mit `os.path.abspath()` können wir aus einem relativen Pfad einen absoluten Pfad erzeugen:

```python
import os

relative_path = os.path.join("data", "measurements.csv")
absolute_path = os.path.abspath(relative_path)

print("Relativ:", relative_path)
print("Absolut:", absolute_path)
```

Umgekehrt kann `os.path.relpath()` einen Pfad relativ zu einem Ausgangspunkt darstellen:

```python
import os

absolute_path = os.path.abspath(
    os.path.join("data", "measurements.csv")
)

relative_path = os.path.relpath(
    absolute_path,
    start=os.getcwd(),
)

print(relative_path)
```

Hier wird also gefragt:

> Wie lautet der Pfad zu `absolute_path`, wenn wir bei `os.getcwd()` starten?

## Direkte Inhalte eines Ordners mit `listdir()`

Mit `os.listdir()` erhalten wir die **direkten Namen** in einem Ordner:

```python
import os

entries = os.listdir(".")

print(entries)
```

Der Punkt `"."` bedeutet dabei: aktueller Arbeitsordner.

Wichtig: `listdir()` liefert nur die **Namen der direkten Einträge** zurück. Es sagt noch nicht, ob ein Eintrag eine Datei oder ein Unterordner ist.

Angenommen `entries` enthält:

```text
["data", "analysis.py", "README.md"]
```

Dann ist `"analysis.py"` zunächst nur ein Name.

Um zu prüfen, ob es wirklich eine Datei ist, brauchen wir den vollständigen Pfad:

```python
import os

entries = os.listdir(".")

for name in entries:
    full_path = os.path.join(".", name)

    if os.path.isfile(full_path):
        print(name, "ist eine Datei")
```

Das Zusammenspiel ist also:

```text
listdir() → Name
join()    → vollständiger Pfad
isfile()  → prüfen, ob dieser Pfad eine Datei bezeichnet
```

## Dateien nach ihrer Endung auswählen

Häufig wollen wir nur Dateien mit einer bestimmten Endung betrachten, zum Beispiel CSV-Dateien.

Für einen einzelnen Dateinamen:

```python
filename = "measurements.CSV"

if filename.lower().endswith(".csv"):
    print("CSV-Dateiname gefunden")
```

Durch `.lower()` werden auch Schreibweisen wie

```text
measurements.CSV
measurements.Csv
measurements.csv
```

erkannt.

In Kombination mit `listdir()`:

```python
import os

for name in os.listdir("."):
    full_path = os.path.join(".", name)

    if os.path.isfile(full_path) and name.lower().endswith(".csv"):
        print("CSV-Datei:", full_path)
```

Dabei prüfen wir zwei verschiedene Dinge:

1. `os.path.isfile(full_path)`  
   → Der Pfad bezeichnet tatsächlich eine Datei.

2. `name.lower().endswith(".csv")`  
   → Der Dateiname endet auf `.csv`.

### Eine `.csv`-Endung garantiert noch keinen gültigen CSV-Inhalt

Ein Name wie

```text
not_really_data.csv
```

sagt nur etwas über den **Dateinamen** aus.

Die Datei könnte trotzdem ungültige oder völlig andere Inhalte enthalten.

Zum Beispiel könnte darin stehen:

```text
Dies ist eigentlich gar keine Tabelle.
```

Die Prüfung

```python
filename.lower().endswith(".csv")
```

antwortet also nur auf die Frage:

> Sieht der Dateiname wie ein CSV-Dateiname aus?

Ob der Inhalt tatsächlich sinnvoll als CSV gelesen werden kann, muss später beim Einlesen beziehungsweise Verarbeiten geprüft werden.

## Unterordner mit `os.walk()` durchsuchen

`os.listdir()` betrachtet nur **einen Ordner**.

Wenn auch Unterordner durchsucht werden sollen, können wir `os.walk()` verwenden.

<!-- pytest-codeblocks:skip -->

```python
import os

for root, dirs, files in os.walk("."):
    print("Aktueller Ordner:", root)
    print("Unterordner:", dirs)
    print("Dateien:", files)
```

Bei jedem Schritt liefert `os.walk()` drei Dinge:

- `root`: der Ordner, der gerade betrachtet wird
- `dirs`: die Namen seiner direkten Unterordner
- `files`: die Namen seiner direkten Dateien

Wichtig ist wieder: Die Einträge in `files` sind zunächst nur **Dateinamen**.

Um daraus einen verwendbaren Pfad zu machen, verbinden wir `root` und Dateiname:

<!-- pytest-codeblocks:skip -->

```python
import os

for root, dirs, files in os.walk("."):
    for filename in files:
        full_path = os.path.join(root, filename)
        print(full_path)
```

Damit lassen sich zum Beispiel alle CSV-Dateinamen in einem ganzen Ordnerbaum finden:

<!-- pytest-codeblocks:skip -->

```python
import os

for root, dirs, files in os.walk("."):
    for filename in files:
        if filename.lower().endswith(".csv"):
            full_path = os.path.join(root, filename)
            print(full_path)
```

Hier brauchen wir normalerweise kein zusätzliches `isfile()`, weil `files` von `os.walk()` bereits die Dateinamen des jeweiligen Ordners enthält.

## `listdir()` oder `walk()`?

Die beiden Funktionen beantworten unterschiedliche Fragen.

### Nur einen Ordner betrachten

```python
os.listdir(folder)
```

liefert die direkten Einträge in genau diesem Ordner.

### Einen ganzen Ordnerbaum durchsuchen

```python
os.walk(folder)
```

läuft zusätzlich durch Unterordner.

Als Faustregel:

- **nur ein Ordner** → `listdir()`
- **Ordner und Unterordner** → `walk()`

## Ein kleines Gesamtbeispiel

Angenommen wir möchten alle CSV-Dateien unterhalb des Ordners `data` finden.

<!-- pytest-codeblocks:skip -->

```python
import os

start_folder = "data"

for root, dirs, files in os.walk(start_folder):
    for filename in files:
        if filename.lower().endswith(".csv"):
            full_path = os.path.join(root, filename)
            print(full_path)
```

Der Ablauf ist dabei:

1. `os.walk(start_folder)` durchläuft alle Ordner.
2. `files` enthält die Dateinamen im aktuellen `root`.
3. `.lower().endswith(".csv")` filtert passende Namen.
4. `os.path.join(root, filename)` erzeugt den vollständigen Pfad.

## Zusammenfassung

Die wichtigsten Werkzeuge aus diesem Kapitel sind:

```python
import os

os.getcwd()

os.path.join("data", "file.csv")
os.path.abspath("data/file.csv")
os.path.relpath("/some/absolute/path")

os.listdir(".")
os.path.isfile("file.csv")

"file.csv".lower().endswith(".csv")

os.walk(".")
```

Dabei ist besonders wichtig, zwischen **Namen**, **Pfaden** und **Dateiinhalten** zu unterscheiden:

- `"measurements.csv"` kann nur ein Dateiname sein.
- `os.path.join(root, "measurements.csv")` ergibt einen Pfad.
- `os.path.isfile(...)` prüft, ob dieser Pfad eine Datei bezeichnet.
- `.endswith(".csv")` prüft nur die Endung des Namens.
- Ob der Inhalt tatsächlich gültige CSV-Daten enthält, ist eine separate Frage.
