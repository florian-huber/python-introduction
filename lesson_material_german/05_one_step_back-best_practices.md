# Python-Umgebungen, Packages und `uv`

Python selbst bringt bereits viele nützliche Module mit. `random`, `math`, `pathlib` oder `csv` gehören zum Beispiel zur **Standardbibliothek** und stehen nach einer normalen Python-Installation direkt zur Verfügung.

Für Data Science brauchen wir aber häufig zusätzliche Bibliotheken wie **NumPy**, **pandas**, **matplotlib** oder **scikit-learn**. Diese sind nicht automatisch Bestandteil von Python und müssen für das jeweilige Projekt installiert werden.

In diesem Kapitel geht es deshalb vor allem um drei Fragen:

1. Wo werden zusätzliche Python-Packages installiert?
2. Wie verhindern wir, dass sich verschiedene Projekte gegenseitig beeinflussen?
3. Wie verwalten wir diese Pakete mit `uv`?

## Standardbibliothek oder externes Package?

Ein Modul aus der Standardbibliothek können wir direkt importieren:

```python
import random

print(random.randint(1, 6))
```

Bei einem externen Package funktioniert der Import erst, wenn es in der verwendeten Python-Umgebung installiert wurde.

Zum Beispiel:

```python
import numpy as np

print(np.array([1, 2, 3]))
```

Falls NumPy in der aktuellen Umgebung fehlt, erhalten wir typischerweise:

```text
ModuleNotFoundError: No module named 'numpy'
```

Viele externe Python-Packages werden über den **Python Package Index (PyPI)** veröffentlicht. PyPI ist der Paket-Index; ein Tool wie `uv` lädt passende Versionen herunter und installiert sie.

## Warum virtuelle Umgebungen?

Stellen wir uns zwei Projekte vor:

```text
projekt_a/
projekt_b/
```

Beide verwenden Python, benötigen aber möglicherweise unterschiedliche Bibliotheken oder unterschiedliche Versionen derselben Bibliothek.

Wenn alle Packages global in dieselbe Python-Installation installiert würden, könnten sich Projekte gegenseitig beeinflussen.

Eine **virtuelle Umgebung** (*virtual environment*) löst dieses Problem. Sie enthält eine projektspezifische Python-Umgebung mit den dort installierten Packages.

Typischerweise heißt dieser Ordner:

```text
.venv/
```

Die `.venv` gehört zum lokalen Arbeitsverzeichnis und sollte normalerweise **nicht in Git eingecheckt** werden.

## `uv`

Im Kurs verwenden wir **uv** zum Verwalten von Python-Versionen, virtuellen Umgebungen und Abhängigkeiten.

Prüfen, ob `uv` verfügbar ist:

```bash
uv --version
```

Die offizielle Dokumentation befindet sich unter:

<https://docs.astral.sh/uv/>

`uv` kann auch Python-Versionen verwalten. Zum Beispiel:

```bash
uv python install 3.14
```

und:

```bash
uv python list
```

Eine Python-Version kann für ein Projekt festgehalten werden:

```bash
uv python pin 3.14
```

Dadurch entsteht eine Datei namens `.python-version`.

---

## Variante 1: Eine einfache virtuelle Umgebung

Für kleine Übungen oder einen Ordner mit einzelnen Skripten reicht oft eine normale virtuelle Umgebung.

Im Projektordner:

```bash
uv venv --python 3.14
```

Dadurch wird eine `.venv` erzeugt.

### Packages installieren

Nun können wir zum Beispiel NumPy installieren:

```bash
uv pip install numpy
```

Mehrere Packages können gleichzeitig installiert werden:

```bash
uv pip install numpy pandas matplotlib
```

`uv pip install` sucht im aktuellen Ordner automatisch nach einer passenden virtuellen Umgebung, insbesondere nach `.venv`.

Welche Packages dort installiert sind:

```bash
uv pip list
```

Danach können sie in Python importiert werden:

```python
import numpy as np
import pandas as pd

print(np.arange(5))
print(pd.DataFrame({"value": [10, 20, 30]}))
```

### Umgebung aktivieren

Eine virtuelle Umgebung kann auch klassisch aktiviert werden.

macOS/Linux:

```bash
source .venv/bin/activate
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Danach beziehen sich `python` und andere installierte Programme in diesem Terminal auf diese Umgebung.

Mit:

```bash
python --version
```

kann überprüft werden, welcher Python-Interpreter gerade verwendet wird.

In VS Code sollte ebenfalls der Interpreter aus `.venv` ausgewählt werden. Das geht über:

```text
Python: Select Interpreter
```

### Eine Umgebung kann neu erzeugt werden

Die `.venv` enthält installierte Dateien, aber nicht den eigentlichen Quellcode des Projekts.

Wenn eine Umgebung kaputtgeht oder gelöscht wird, ist das normalerweise kein Problem: Sie kann neu erstellt und die benötigten Packages können erneut installiert werden.

Genau deshalb ist es wichtig, die **Abhängigkeiten eines Projekts separat zu dokumentieren**.

---

## Variante 2: Ein uv-Projekt

Für ein richtiges Softwareprojekt ist es praktischer, wenn uv nicht nur die Umgebung verwaltet, sondern auch speichert, welche Packages das Projekt benötigt.

Ein neues Projekt kann mit:

```bash
uv init
```

initialisiert werden.

Dabei entsteht unter anderem eine Datei:

```text
pyproject.toml
```

Sie beschreibt das Python-Projekt und enthält auch dessen direkte Abhängigkeiten.

### Eine Dependency hinzufügen

NumPy zum Projekt hinzufügen:

```bash
uv add numpy
```

Mehrere Bibliotheken:

```bash
uv add numpy pandas matplotlib
```

`uv add` macht mehrere Dinge:

1. Das Package wird als Dependency in `pyproject.toml` eingetragen.
2. Die passenden Versionen aller benötigten Dependencies werden bestimmt.
3. Die Projektumgebung `.venv` wird aktualisiert.
4. Die aufgelösten Versionen werden in `uv.lock` festgehalten.

Für ein uv-Projekt ist deshalb `uv add` normalerweise die bessere Wahl als ein manuelles `uv pip install`.

Ein Ausschnitt aus einer `pyproject.toml` könnte dann ungefähr so aussehen:

```toml
[project]
name = "data-analysis"
version = "0.1.0"
dependencies = [
    "matplotlib",
    "numpy",
    "pandas",
]
```

Die genaue Reihenfolge und zusätzliche Einträge können abweichen.

## `uv.lock`

Neben `pyproject.toml` erzeugt uv eine Datei namens:

```text
uv.lock
```

Der Unterschied ist vereinfacht:

- `pyproject.toml` beschreibt, **welche direkten Abhängigkeiten** unser Projekt haben soll.
- `uv.lock` hält fest, **welche konkreten Versionen** für dieses Projekt aufgelöst wurden.

Beide Dateien gehören normalerweise ins Git-Repository.

Die `.venv` dagegen nicht.

Eine typische Struktur kann also so aussehen:

```text
my_project/
├── .venv/
├── .python-version
├── pyproject.toml
├── uv.lock
├── analysis.py
└── README.md
```

## Python-Code mit `uv run` ausführen

In einem uv-Projekt können wir ein Skript direkt in der passenden Projektumgebung starten:

```bash
uv run python analysis.py
```

Oder kürzer:

```bash
uv run analysis.py
```

`uv run` prüft dabei automatisch, ob die Umgebung zu `pyproject.toml` und `uv.lock` passt, und synchronisiert sie bei Bedarf.

Das ist besonders praktisch, weil die Umgebung vorher nicht manuell aktiviert werden muss.

Beispiel `analysis.py`:

```python
import numpy as np
import pandas as pd

values = np.arange(1, 6)

table = pd.DataFrame({
    "value": values,
    "squared": values ** 2,
})

print(table)
```

Start:

```bash
uv run python analysis.py
```

## Ein vorhandenes Projekt einrichten

Wenn ein Repository bereits `pyproject.toml` und `uv.lock` enthält, müssen die Packages nicht einzeln von Hand installiert werden.

Stattdessen:

```bash
uv sync
```

`uv sync` erzeugt beziehungsweise aktualisiert die `.venv` so, dass sie zum Projekt passt.

Ein typischer Ablauf nach dem Klonen eines Projekts ist deshalb:

```bash
git clone ...
cd projektname
uv sync
uv run python analysis.py
```

## Packages entfernen oder ergänzen

Eine Dependency entfernen:

```bash
uv remove matplotlib
```

Eine weitere hinzufügen:

```bash
uv add seaborn
```

Eine bestimmte Versionsanforderung kann ebenfalls angegeben werden:

```bash
uv add "numpy>=2,<3"
```

Solche Versionsgrenzen sind vor allem dann wichtig, wenn ein Projekt nur mit bestimmten Bibliotheksversionen kompatibel ist.

## Abhängigkeiten anschauen

Installierte Packages der Umgebung:

```bash
uv pip list
```

Die Abhängigkeitsstruktur eines uv-Projekts kann außerdem mit:

```bash
uv tree
```

angezeigt werden.

Dabei sieht man, dass ein direkt installiertes Package häufig wiederum weitere Packages benötigt.

Diese indirekt benötigten Packages heißen **Dependencies** oder Abhängigkeiten.

## Was sollte in Git?

Für ein uv-Projekt ist als Faustregel sinnvoll:

**In Git:**

```text
pyproject.toml
uv.lock
.python-version
eigener Python-Code
README.md
```

**Nicht in Git:**

```text
.venv/
__pycache__/
```

Die virtuelle Umgebung wird lokal neu erzeugt. `pyproject.toml` und `uv.lock` enthalten die Informationen, die dafür benötigt werden.

## `uv pip install` oder `uv add`?

Beide Befehle installieren Packages, aber sie haben unterschiedliche Rollen.

### `uv pip install`

Gut geeignet für:

- eine einfache virtuelle Umgebung,
- schnelle Experimente,
- einen Ordner ohne uv-Projektverwaltung.

Beispiel:

```bash
uv venv
uv pip install numpy pandas
```

### `uv add`

Geeignet für ein uv-Projekt:

```bash
uv init
uv add numpy pandas
```

Hier werden die Dependencies zusätzlich dauerhaft im Projekt dokumentiert.

Für länger laufende Kurs- und Softwareprojekte verwenden wir deshalb bevorzugt:

```text
uv init
uv add ...
uv run ...
uv sync
```

## Ein kurzer typischer Workflow

Ein neues Projekt:

```bash
mkdir my_analysis
cd my_analysis

uv init
uv python pin 3.14
uv add numpy pandas matplotlib
```

Dann eine Datei `analysis.py` schreiben und starten:

```bash
uv run python analysis.py
```

Später kommt eine weitere Bibliothek dazu:

```bash
uv add seaborn
```

Jemand anderes klont das Projekt:

```bash
git clone ...
cd my_analysis
uv sync
uv run python analysis.py
```

Damit haben beide Personen eine reproduzierbare Projektumgebung, ohne den kompletten `.venv`-Ordner austauschen zu müssen.

---

## Ein kleiner Rest "Best Practice"

An dieser Stelle brauchen wir noch keine ausführliche Clean-Code-Diskussion. Zwei Dinge lohnen sich aber schon jetzt:

1. Verwendet verständliche Namen für Variablen, Funktionen und Dateien.
2. Trennt euren eigenen Code von der technischen Projektumgebung.

Zum zweiten Punkt gehört insbesondere:

- eigener Code kommt ins Repository,
- Dependencies werden in `pyproject.toml` und `uv.lock` beschrieben,
- `.venv` wird lokal erzeugt und nicht geteilt.

Später im Kurs werden wir genauer darauf eingehen, wie Code strukturiert, getestet, formatiert und langfristig wartbar gemacht wird.
