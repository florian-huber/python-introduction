# Navigieren durch Ordner, Pfade und CSV-Dateien

In diesem Kapitel arbeiten wir direkt mit einer vorbereiteten Ordnerstruktur. Sie liegt im Kursprojekt unter:

```text
data/folder-handling/
```

Die wichtigsten Teile sehen so aus:

```text
folder-handling/
├── input/
│   ├── archive/
│   │   ├── 2025/
│   │   │   └── team_old.csv
│   │   └── 2026/
│   │       └── team_new.csv
│   ├── bad_number.csv
│   ├── broken_parser.csv
│   ├── empty.csv
│   ├── latin1.csv
│   ├── photo.jpg
│   ├── README.txt
│   ├── team_alpha.csv
│   └── team_beta.CSV
├── input_invalid_only/
└── output/
    └── combined_scores.csv
```

Wir verwenden zunächst nur `os` und `os.path`. Später lesen wir die CSV-Dateien mit `pandas`.

---

## 1. Wo arbeitet Python gerade?

Python hat immer einen aktuellen **Arbeitsordner** (*current working directory*).

```python
import os

print(os.getcwd())
```

Alle **relativen Pfade** werden von diesem Arbeitsordner aus interpretiert.

Wenn wir das Kursrepository geöffnet haben, können wir zum Beispiel schreiben:

<!-- pytest-codeblocks:skip -->
```python
import os

base_folder = os.path.join("data", "folder-handling")
input_folder = os.path.join(base_folder, "input")

print(input_folder)
```

Das ist ein relativer Pfad.

Mit `os.path.abspath()` können wir uns den dazugehörigen absoluten Pfad anzeigen lassen:

<!-- pytest-codeblocks:skip -->
```python
print(os.path.abspath(input_folder))
```

Ein **relativer Pfad** hängt vom aktuellen Arbeitsordner ab. Ein **absoluter Pfad** beschreibt den vollständigen Ort im Dateisystem.

---

## 2. Pfade mit `join()` zusammensetzen

Pfade sollten nicht per Hand mit `/` oder `\` zusammengesetzt werden.

Stattdessen:
<!-- pytest-codeblocks:skip -->
```python
import os

base_folder = os.path.join("data", "folder-handling")
input_folder = os.path.join(base_folder, "input")

file_path = os.path.join(input_folder, "team_alpha.csv")

print(file_path)
```

`os.path.join()` verwendet automatisch den passenden Pfadtrenner des Betriebssystems.

Wir können uns auch wieder einen Pfad **relativ zu einem bestimmten Ausgangspunkt** anzeigen lassen:

<!-- pytest-codeblocks:skip -->

```python
absolute_path = os.path.abspath(file_path)

relative_path = os.path.relpath(
    absolute_path,
    start=input_folder,
)

print(relative_path)
```

Das Ergebnis ist hier:

```text
team_alpha.csv
```

Später verwenden wir genau diese Idee, um im Ergebnis nicht vollständige absolute Pfade zu speichern, sondern nur sinnvolle relative Dateinamen.

---

## 3. Was liegt direkt in einem Ordner?

Mit `os.listdir()` erhalten wir die **direkten Namen** in einem Ordner:

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join("data", "folder-handling", "input")

entries = os.listdir(input_folder)

print(entries)
```

Die Reihenfolge kann sich zwischen Betriebssystemen unterscheiden.

Wichtig: `listdir()` liefert zunächst nur **Namen**.

Zum Beispiel:

```text
team_alpha.csv
archive
README.txt
```

Ein Name allein sagt noch nicht, ob es sich um eine Datei oder einen Ordner handelt.

---

## 4. Datei oder Ordner?

Für eine Prüfung mit `os.path.isfile()` brauchen wir wieder einen vollständigen Pfad:

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join("data", "folder-handling", "input")

for name in os.listdir(input_folder):
    full_path = os.path.join(input_folder, name)

    if os.path.isfile(full_path):
        print(name, "-> Datei")
    else:
        print(name, "-> keine Datei")
```

Das Muster ist wichtig:

```text
listdir()  -> Name
join()     -> Pfad
isfile()   -> prüfen
```

---

## 5. CSV-Dateinamen auswählen

Nun interessieren uns nur CSV-Dateien.

Ein einfacher Filter ist:

<!-- pytest-codeblocks:skip -->
```python
filename = "team_beta.CSV"

print(filename.lower().endswith(".csv"))
```

Durch `.lower()` akzeptieren wir auch Schreibweisen wie:

```text
team_beta.CSV
team_beta.Csv
team_beta.csv
```

Für den Inhalt des Eingabeordners:

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join("data", "folder-handling", "input")

for name in os.listdir(input_folder):
    full_path = os.path.join(input_folder, name)

    if os.path.isfile(full_path) and name.lower().endswith(".csv"):
        print(name)
```

### Endung ist nicht Inhalt

Im Ordner liegt auch:

```text
photo.jpg
```

Wir benutzen diese Datei **nur als Beispiel**:

> Eine Dateiendung ist zunächst nur ein Teil des Dateinamens.

Aus `.jpg` folgt nicht automatisch, dass unser Python-Code weiß, ob der Inhalt wirklich ein gültiges JPEG-Bild ist.

Genauso bedeutet `.csv` zunächst nur:

> Der Dateiname sieht wie ein CSV-Dateiname aus.

Ob die Datei wirklich als CSV gelesen werden kann, erfahren wir erst beim Einlesen.

Wir lesen `photo.jpg` in diesem Kapitel **nicht** als Bild ein.

---

## 6. Unterordner mit `os.walk()` durchsuchen

`os.listdir()` betrachtet nur einen einzelnen Ordner.

Im Beispiel liegen aber weitere CSV-Dateien unter:

```text
input/archive/2025/
input/archive/2026/
```

Dafür verwenden wir `os.walk()`.

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join("data", "folder-handling", "input")

for root, dirs, files in os.walk(input_folder):
    print("root:", root)
    print("dirs:", dirs)
    print("files:", files)
    print()
```

Bei jedem Schritt bekommen wir:

- `root`: den Ordner, der gerade betrachtet wird
- `dirs`: die direkten Unterordner in diesem Ordner
- `files`: die direkten Dateinamen in diesem Ordner

Die Einträge in `files` sind wieder nur Dateinamen.

Den vollständigen Pfad erhalten wir mit:

<!-- pytest-codeblocks:skip -->
```python
full_path = os.path.join(root, filename)
```

Zum Beispiel:

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join("data", "folder-handling", "input")

for root, dirs, files in os.walk(input_folder):
    for filename in files:
        if filename.lower().endswith(".csv"):
            full_path = os.path.join(root, filename)
            print(full_path)
```

Damit finden wir auch CSV-Dateien in Unterordnern.

---

# CSV-Dateien einlesen

Ab hier verwenden wir zusätzlich `pandas`.

```python
import pandas as pd
```

## 7. Eine gültige UTF-8-CSV lesen

Beginnen wir mit einer bekannten gültigen Datei:

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

input_folder = os.path.join("data", "folder-handling", "input")
file_path = os.path.join(input_folder, "team_alpha.csv")

data = pd.read_csv(file_path, encoding="utf-8")

print(data)
```

Die Datei enthält Namen mit Umlauten beziehungsweise anderen Unicode-Zeichen.

Mit:

<!-- pytest-codeblocks:skip -->
```python
encoding="utf-8"
```

legen wir ausdrücklich fest, welche Zeichenkodierung wir erwarten.

---

## 8. Nicht jede `.csv`-Datei ist gültig

Im Eingabeordner liegen absichtlich verschiedene Problemfälle:

```text
empty.csv
broken_parser.csv
latin1.csv
bad_number.csv
```

Wir testen sie einzeln.

### Leere Datei

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

path = os.path.join(
    "data", "folder-handling", "input", "empty.csv"
)

data = pd.read_csv(path, encoding="utf-8")
```

Pandas löst hier einen:

```text
pandas.errors.EmptyDataError
```

aus.

### Kaputte CSV-Struktur

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

path = os.path.join(
    "data", "folder-handling", "input", "broken_parser.csv"
)

data = pd.read_csv(path, encoding="utf-8")
```

Hier ist die CSV-Struktur absichtlich beschädigt.

Das führt zu einem:

```text
pandas.errors.ParserError
```

### Falsche Zeichenkodierung

`latin1.csv` wurde absichtlich **nicht** als UTF-8 gespeichert.

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

path = os.path.join(
    "data", "folder-handling", "input", "latin1.csv"
)

data = pd.read_csv(path, encoding="utf-8")
```

Beim Versuch, sie als UTF-8 zu lesen, entsteht ein:

```text
UnicodeDecodeError
```

### Nicht-numerischer Wert

`bad_number.csv` kann zunächst problemlos als CSV gelesen werden:

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

path = os.path.join(
    "data", "folder-handling", "input", "bad_number.csv"
)

data = pd.read_csv(path, encoding="utf-8")

print(data)
```

Aber in der Spalte `score` steht ein Textwert.

Wenn wir ausdrücklich eine numerische Spalte verlangen:

<!-- pytest-codeblocks:skip -->
```python
data["score"] = pd.to_numeric(
    data["score"],
    errors="raise",
)
```

entsteht ein:

```text
ValueError
```

Das ist ein anderes Problem als eine kaputte CSV-Datei:

- Die Datei konnte gelesen werden.
- Aber ein bestimmter Wert entspricht nicht unserer Erwartung.

---

# Dateien robust verarbeiten

## 9. Eine Datei mit `try`/`except` laden

Jetzt kombinieren wir die bekannten Fehlertypen.

<!-- pytest-codeblocks:skip -->
```python
import pandas as pd


def load_csv(file_path):
    try:
        data = pd.read_csv(file_path, encoding="utf-8")
        data["score"] = pd.to_numeric(
            data["score"],
            errors="raise",
        )
        return data

    except OSError as error:
        print("OS-Fehler:", file_path, error)

    except UnicodeDecodeError:
        print("Falsche Kodierung:", file_path)

    except pd.errors.ParserError:
        print("CSV konnte nicht geparst werden:", file_path)

    except pd.errors.EmptyDataError:
        print("CSV ist leer:", file_path)

    except ValueError:
        print("Ungültiger Zahlenwert:", file_path)

    return None
```

Damit behandeln wir gezielt:

- OS-Fehler
- Kodierungsfehler
- Parserfehler
- leere Dateien
- ungültige Zahlenwerte

Ein **OS-Fehler** kann zum Beispiel auftreten, wenn eine Datei zwischenzeitlich gelöscht wurde oder nicht zugreifbar ist.

---

## 10. Fehlerhafte Datei überspringen und weitermachen

Nun durchlaufen wir alle CSV-Dateien.

Eine fehlerhafte Datei soll **nicht das ganze Programm stoppen**.

<!-- pytest-codeblocks:skip -->
```python
import os

input_folder = os.path.join(
    "data", "folder-handling", "input"
)

for root, dirs, files in os.walk(input_folder):
    for filename in files:
        if not filename.lower().endswith(".csv"):
            continue

        file_path = os.path.join(root, filename)

        data = load_csv(file_path)

        if data is None:
            continue

        print("Erfolgreich:", filename)
        print(data)
```

Das wichtige Muster innerhalb der Schleife ist:

```text
data = load_csv(file_path)

if data is None:
    continue
```

Eine fehlerhafte Datei wird ausgelassen.

Danach läuft die Schleife mit der **nächsten Datei** weiter.

---

# Mehrere Dateien zu einem Ergebnis zusammenführen

## 11. Relative Dateinamen speichern

Für das spätere Ergebnis möchten wir wissen, aus welcher Datei jede Zeile stammt.

Aber wir wollen keinen langen absoluten Pfad wie:

```text
/home/.../python-introduction/data/folder-handling/input/archive/2025/team_old.csv
```

speichern.

Stattdessen soll im Ergebnis zum Beispiel stehen:

```text
team_alpha.csv
archive/2025/team_old.csv
```

Dafür verwenden wir:

<!-- pytest-codeblocks:skip -->
```python
relative_name = os.path.relpath(
    file_path,
    start=input_folder,
)
```

Damit bleibt der Dateiname relativ zur Eingabemappe.

---

## 12. Gültige Dateien sammeln

Jetzt kombinieren wir alles.

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

input_folder = os.path.join(
    "data", "folder-handling", "input"
)

tables = []

for root, dirs, files in os.walk(input_folder):
    for filename in files:
        if not filename.lower().endswith(".csv"):
            continue

        file_path = os.path.join(root, filename)
        data = load_csv(file_path)

        if data is None:
            continue

        relative_name = os.path.relpath(
            file_path,
            start=input_folder,
        )

        data["source_file"] = relative_name
        tables.append(data)
```

`tables` enthält anschließend nur erfolgreich gelesene Tabellen.

---

## 13. Ausgabe außerhalb der Eingabemappe speichern

Die Ergebnisdatei soll **nicht** in `input/` liegen.

Warum?

Wenn wir später erneut durch `input/` laufen, soll unsere eigene Ergebnisdatei nicht plötzlich wieder als Eingabedatei auftauchen.

Darum verwenden wir:

```text
folder-handling/output/
```

Die Ausgabe liegt also bewusst **außerhalb der Eingabemappe**.

<!-- pytest-codeblocks:skip -->
```python
import os
import pandas as pd

base_folder = os.path.join(
    "data", "folder-handling"
)

output_path = os.path.join(
    base_folder,
    "output",
    "combined_scores.csv",
)

if tables:
    result = pd.concat(tables, ignore_index=True)
else:
    result = pd.DataFrame(
        columns=["name", "city", "score", "source_file"]
    )

result.to_csv(
    output_path,
    index=False,
    encoding="utf-8",
)

print("Gespeichert:", output_path)
```

Im vorbereiteten Ordner existiert `combined_scores.csv` bereits.

`to_csv()` überschreibt diese Datei standardmäßig.

Das ist hier absichtlich sichtbar:

> Wenn unter `output_path` bereits eine Datei existiert, wird ihr bisheriger Inhalt ersetzt.

Deshalb sollten Ein- und Ausgabepfade bewusst gewählt werden.

---

## 14. Was passiert bei null gültigen Dateien?

Auch diesen Fall sollten wir testen.

Dafür gibt es den vorbereiteten Ordner:

```text
data/folder-handling/input_invalid_only/
```

Dort liegen nur Dateien, die bei unserer Verarbeitung scheitern.

Setzt:

<!-- pytest-codeblocks:skip -->
```python
input_folder = os.path.join(
    "data",
    "folder-handling",
    "input_invalid_only",
)
```

und führt die Sammellogik erneut aus.

Da keine gültige Datei gelesen wird, bleibt:

<!-- pytest-codeblocks:skip -->
```python
tables = []
```

Dann erzeugen wir trotzdem:

<!-- pytest-codeblocks:skip -->
```python
result = pd.DataFrame(
    columns=["name", "city", "score", "source_file"]
)
```

und speichern sie mit:

<!-- pytest-codeblocks:skip -->
```python
result.to_csv(output_path, index=False, encoding="utf-8")
```

Die Ergebnisdatei enthält dann **keine Datenzeilen**, aber weiterhin ihre Kopfzeile:

```text
name,city,score,source_file
```

Das ist häufig deutlich hilfreicher als überhaupt keine Ergebnisdatei zu erzeugen.

---

# Komplettes Beispiel

Der folgende Code kombiniert die wesentlichen Schritte.

<!-- pytest-codeblocks:skip -->
```python
import os

import pandas as pd


def load_csv(file_path):
    try:
        data = pd.read_csv(file_path, encoding="utf-8")

        data["score"] = pd.to_numeric(
            data["score"],
            errors="raise",
        )

        return data

    except OSError as error:
        print("OS-Fehler:", file_path, error)

    except UnicodeDecodeError:
        print("Falsche Kodierung:", file_path)

    except pd.errors.ParserError:
        print("Parserfehler:", file_path)

    except pd.errors.EmptyDataError:
        print("Leere Datei:", file_path)

    except ValueError:
        print("Ungültiger Zahlenwert:", file_path)

    return None


base_folder = os.path.join(
    "data",
    "folder-handling",
)

input_folder = os.path.join(
    base_folder,
    "input",
)

output_path = os.path.join(
    base_folder,
    "output",
    "combined_scores.csv",
)

tables = []

for root, dirs, files in os.walk(input_folder):
    for filename in files:
        if not filename.lower().endswith(".csv"):
            continue

        file_path = os.path.join(root, filename)

        data = load_csv(file_path)

        if data is None:
            continue

        relative_name = os.path.relpath(
            file_path,
            start=input_folder,
        )

        data["source_file"] = relative_name
        tables.append(data)


if tables:
    result = pd.concat(
        tables,
        ignore_index=True,
    )
else:
    result = pd.DataFrame(
        columns=[
            "name",
            "city",
            "score",
            "source_file",
        ]
    )


result.to_csv(
    output_path,
    index=False,
    encoding="utf-8",
)

print()
print("Erfolgreiche Tabellen:", len(tables))
print("Ergebniszeilen:", len(result))
print("Ausgabe:", output_path)
```

Dabei werden fehlerhafte Dateien gemeldet und ausgelassen. Danach werden weitere Dateien trotzdem verarbeitet.

---

## Zusammenfassung

Die wichtigsten Muster sind:

<!-- pytest-codeblocks:skip -->
```python
import os

os.getcwd()

os.path.join(...)
os.path.abspath(...)
os.path.relpath(...)

os.listdir(...)
os.path.isfile(...)

filename.lower().endswith(".csv")

os.walk(...)
```

Für CSV-Dateien zusätzlich:

<!-- pytest-codeblocks:skip -->
```python
import pandas as pd

pd.read_csv(path, encoding="utf-8")
```

und bei einer robusten Verarbeitung unter anderem:

```text
OSError
UnicodeDecodeError
pandas.errors.ParserError
pandas.errors.EmptyDataError
ValueError
```

Wichtig ist die Trennung verschiedener Fragen:

1. **Wie heißt der Eintrag?**
2. **Wo liegt er?**
3. **Ist es tatsächlich eine Datei?**
4. **Hat der Name die gewünschte Endung?**
5. **Kann der Inhalt erfolgreich eingelesen werden?**
6. **Entsprechen die gelesenen Werte den erwarteten Datentypen?**

Genau diese Schritte machen Dateiverarbeitung nachvollziehbar und robust.
