# Python-Umgebungen, Packages und `uv`

Python selbst bringt bereits viele nützliche Module mit. `random`, `math` oder `sys` gehören zum Beispiel zur **Standardbibliothek** und stehen nach einer normalen Python-Installation direkt zur Verfügung.

Daneben gibt es sehr viele zusätzliche Python-Packages, die nicht automatisch mit Python installiert werden. Solche Packages können wir für einzelne Projekte installieren. Wichtig ist dabei vor allem, **wo** sie installiert werden und wie wir später nachvollziehen können, welche Packages zu einem Projekt gehören.

In diesem Kapitel geht es deshalb vor allem um drei Fragen:

1. Was ist eine virtuelle Python-Umgebung?
2. Wie installieren wir zusätzliche Packages mit `uv`?
3. Wie halten wir die Abhängigkeiten eines Projekts nachvollziehbar fest?

## Welche Python-Umgebung benutze ich gerade?

Wenn mehrere Python-Versionen oder virtuelle Umgebungen auf einem Rechner vorhanden sind, ist nicht immer sofort offensichtlich, welcher Interpreter gerade verwendet wird.

Mit der Standardbibliothek können wir uns den verwendeten Interpreter anzeigen lassen:

```python
import sys

print(sys.executable)
print(sys.version)
```

`sys.executable` zeigt den Pfad zu dem Python-Interpreter, mit dem das aktuelle Programm ausgeführt wird.

Auch im Terminal können wir die Python-Version prüfen:

```bash
python --version
```

Je nach Betriebssystem kann stattdessen auch `python3 --version` nötig sein.

## Warum virtuelle Umgebungen?

Stellen wir uns zwei verschiedene Projekte vor:

```text
projekt_a/
projekt_b/
```

Beide verwenden Python, benötigen aber möglicherweise unterschiedliche zusätzliche Packages oder unterschiedliche Versionen derselben Packages.

Wenn alles global in dieselbe Python-Installation installiert würde, könnten sich verschiedene Projekte gegenseitig beeinflussen.

Eine **virtuelle Umgebung** (*virtual environment*) löst dieses Problem. Sie enthält eine eigene Python-Umgebung für ein bestimmtes Projekt.

Typischerweise liegt sie im Projektordner und heißt:

```text
.venv/
```

Die `.venv` enthält installierte Packages und technische Dateien der Umgebung. Sie gehört normalerweise **nicht in Git** und kann jederzeit neu erzeugt werden.

## `uv`

Im Kurs verwenden wir **uv**, um Python-Versionen, virtuelle Umgebungen und externe Packages zu verwalten.

Prüfen, ob `uv` verfügbar ist:

```bash
uv --version
```

Die offizielle Dokumentation findet sich unter:

<https://docs.astral.sh/uv/>

### Python-Versionen mit uv

`uv` kann bei Bedarf auch Python-Versionen installieren:

```bash
uv python install 3.14
```

Verfügbare beziehungsweise gefundene Python-Versionen können angezeigt werden mit:

```bash
uv python list
```

Für einen Projektordner kann eine gewünschte Version festgehalten werden:

<!-- pytest-codeblocks:skip -->
```bash
uv python pin 3.14
```

Dadurch entsteht eine Datei namens `.python-version`.

---

## Variante 1: Eine einfache virtuelle Umgebung

Für kleine Übungen oder einen Ordner mit einzelnen Python-Dateien reicht oft eine normale virtuelle Umgebung.

Im Projektordner:

<!-- pytest-codeblocks:skip -->
```bash
uv venv --python 3.14
```

Dadurch wird standardmäßig ein Ordner `.venv` angelegt.

Danach kann ein zusätzliches Package installiert werden, zum Beispiel:

```bash
uv pip install requests
```

Für diesen Abschnitt ist nicht wichtig, was `requests` genau macht. Es dient nur als Beispiel für ein Package, das **nicht** zur Python-Standardbibliothek gehört.

Mehrere Packages können gemeinsam installiert werden:

```bash
uv pip install requests rich
```

Welche Packages in der aktuellen Umgebung installiert sind, zeigt:

```bash
uv pip list
```

`uv pip install` sucht automatisch nach einer passenden virtuellen Umgebung, insbesondere nach einer `.venv` im aktuellen Ordner oder in einem übergeordneten Ordner.

### Umgebung aktivieren

Eine virtuelle Umgebung kann klassisch aktiviert werden.

macOS/Linux:

<!-- pytest-codeblocks:skip -->
```bash
source .venv/bin/activate
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Danach beziehen sich `python` und andere Programme im Terminal auf diese Umgebung.

Das kann wieder geprüft werden mit:

```python
import sys

print(sys.executable)
```

In VS Code sollte ebenfalls der Interpreter aus `.venv` ausgewählt sein. Das geht über die Command Palette mit:

```text
Python: Select Interpreter
```

### Eine `.venv` ist austauschbar

Die virtuelle Umgebung enthält nicht den eigentlichen Quellcode des Projekts. Wenn `.venv` gelöscht wird, sollte das Projekt deshalb nicht verloren sein.

Die Umgebung kann neu erzeugt werden:

<!-- pytest-codeblocks:skip -->
```bash
uv venv --python 3.14
```

Danach müssten die benötigten Packages allerdings erneut installiert werden.

Damit kommen wir zu einem wichtigen Problem: **Wo steht eigentlich, welche Packages ein Projekt benötigt?**

---

## Variante 2: Ein uv-Projekt

Für ein länger laufendes Projekt ist es praktischer, wenn die benötigten Packages nicht nur installiert, sondern auch als **Dependencies** des Projekts dokumentiert werden.

Ein Projekt kann mit

<!-- pytest-codeblocks:skip -->
```bash
uv init
```

initialisiert werden.

Dabei entsteht unter anderem eine Datei:

```text
pyproject.toml
```

Diese Datei beschreibt das Python-Projekt und kann auch dessen direkte Abhängigkeiten enthalten.

## Ein Package zum Projekt hinzufügen

In einem uv-Projekt verwenden wir dafür bevorzugt:

<!-- pytest-codeblocks:skip -->
```bash
uv add requests
```

`uv add` trägt das Package in `pyproject.toml` ein und aktualisiert gleichzeitig die Projektumgebung und die Lockdatei.

Ein stark vereinfachter Ausschnitt aus `pyproject.toml` könnte danach so aussehen:

```toml
[project]
dependencies = [
    "requests",
]
```

Weitere Dependencies werden genauso ergänzt:

<!-- pytest-codeblocks:skip -->
```bash
uv add rich
```

Entfernen geht mit:

<!-- pytest-codeblocks:skip -->
```bash
uv remove rich
```

Für ein uv-Projekt ist `uv add` normalerweise die passendere Variante als ein bloßes `uv pip install`, weil die Abhängigkeit damit **Teil der Projektbeschreibung** wird.

## `uv.lock`

Zusätzlich zu `pyproject.toml` verwendet uv eine Datei namens:

```text
uv.lock
```

Vereinfacht gesagt:

- `pyproject.toml` beschreibt, **welche Dependencies das Projekt benötigt**.
- `uv.lock` hält fest, **welche konkreten Paketversionen und abhängigen Pakete dafür aufgelöst wurden**.

Das hilft dabei, auf verschiedenen Rechnern möglichst dieselbe Projektumgebung wiederherzustellen.

`uv.lock` wird von uv automatisch verwaltet. `uv run` und andere Projektbefehle prüfen dabei, ob Projektbeschreibung, Lockdatei und Umgebung zusammenpassen.

## Programme mit `uv run` starten

Ein Python-Programm kann innerhalb der Projektumgebung so gestartet werden:

<!-- pytest-codeblocks:skip -->
```bash
uv run python main.py
```

Oder auch:

<!-- pytest-codeblocks:skip -->
```bash
uv run main.py
```

Der große Vorteil: Die virtuelle Umgebung muss vorher nicht manuell aktiviert werden. `uv run` stellt sicher, dass das Kommando in der Projektumgebung ausgeführt wird.

Angenommen `main.py` enthält nur bereits bekannte Python-Grundlagen:

```python
from random import choice

names = ["Ada", "Grace", "Linus"]
print(choice(names))
```

Dann kann die Datei beispielsweise mit

<!-- pytest-codeblocks:skip -->
```bash
uv run python main.py
```

gestartet werden.

## Ein vorhandenes Projekt einrichten

Wenn ein Repository bereits `pyproject.toml` und `uv.lock` enthält, müssen die Dependencies nicht einzeln von Hand installiert werden.

Im Projektordner genügt:

<!-- pytest-codeblocks:skip -->
```bash
uv sync
```

Damit wird die Projektumgebung an die festgehaltenen Dependencies angepasst.

Ein typischer Ablauf nach dem Klonen eines bestehenden Projekts ist damit:

<!-- pytest-codeblocks:skip -->
```bash
git clone ...
cd projektname
uv sync
uv run python main.py
```

## Abhängigkeiten anschauen

Installierte Packages in der aktuellen Umgebung:

<!-- pytest-codeblocks:skip -->
```bash
uv pip list
```

Die Abhängigkeitsstruktur eines uv-Projekts kann mit

<!-- pytest-codeblocks:skip -->
```bash
uv tree
```

angezeigt werden.

Dabei wird schnell sichtbar, dass ein direkt hinzugefügtes Package oft selbst weitere Packages benötigt. Solche Abhängigkeiten heißen **Dependencies**.

## Was gehört in Git?

Eine typische Projektstruktur könnte so aussehen:

```text
my_project/
├── .venv/
├── .python-version
├── pyproject.toml
├── uv.lock
├── main.py
└── README.md
```

Normalerweise gehören ins Repository:

```text
.python-version
pyproject.toml
uv.lock
eigener Python-Code
README.md
```

Nicht ins Repository gehört typischerweise:

```text
.venv/
__pycache__/
```

Die `.venv` wird lokal neu erzeugt. `pyproject.toml` und `uv.lock` enthalten die relevanten Informationen, um die Umgebung wiederherzustellen.

## `uv pip install` oder `uv add`?

Die beiden Befehle sehen ähnlich aus, erfüllen aber unterschiedliche Rollen.

### Einfache Umgebung

Für einen kleinen Ordner ohne uv-Projektverwaltung:

```bash
uv venv
uv pip install requests
```

### Verwaltetes uv-Projekt

Für ein Projekt, dessen Dependencies dauerhaft dokumentiert werden sollen:

<!-- pytest-codeblocks:skip -->
```bash
uv init
uv add requests
uv run python main.py
```

Der zweite Weg wird mit zunehmender Projektgröße wichtiger, weil die Abhängigkeiten nicht nur auf einem einzelnen Rechner installiert, sondern im Projekt selbst dokumentiert sind.

## Ein typischer kleiner Workflow

Ein neuer Projektordner:

```bash
mkdir my_project
cd my_project
uv init
uv python pin 3.14
uv add requests
```

Danach wird Python-Code geschrieben und zum Beispiel so gestartet:

<!-- pytest-codeblocks:skip -->
```bash
uv run python main.py
```

Wenn später eine weitere externe Bibliothek benötigt wird:

<!-- pytest-codeblocks:skip -->
```bash
uv add rich
```

Auf einem anderen Rechner kann dasselbe Projekt mit

<!-- pytest-codeblocks:skip -->
```bash
uv sync
```

wieder eingerichtet werden.

## Kurz zusammengefasst

Für den Moment sind vor allem diese Unterschiede wichtig:

- Python bringt eine **Standardbibliothek** bereits mit.
- Zusätzliche Packages werden separat installiert.
- Eine `.venv` hält die Umgebung eines Projekts vom Rest des Rechners getrennt.
- `uv pip install ...` installiert Packages in eine Umgebung.
- `uv add ...` fügt eine Dependency zu einem uv-Projekt hinzu.
- `pyproject.toml` und `uv.lock` dokumentieren das Projekt und seine Dependencies.
- `uv run ...` führt Programme in der passenden Projektumgebung aus.
- `uv sync` stellt eine vorhandene Projektumgebung wieder her.

Fragen zu Code-Qualität, Tests, Formatierung und größerer Projektstruktur kommen später im Kurs ausführlicher zurück. Hier geht es zunächst darum, dass Python-Projekte **eine klar definierte und reproduzierbare Umgebung** bekommen.
