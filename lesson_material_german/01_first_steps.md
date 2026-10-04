# Erste Schritte mit Python

In dieser Veranstaltung lernen wir Programmieren mit der Programmiersprache Python. Python wird euch auch im restlichen DAISY-Studium begleiten, denn wir arbeiten in vielen Lehrveranstaltungen und Projekten damit. Für den Kurs richten wir eine einheitliche Python-Version und einen gemeinsamen Editor ein.

## Python installieren

Für diesen Kurs arbeiten wir mit **Python 3.14**. Installiert am besten die jeweils aktuelle Python-3.14.x-Version von der offiziellen Seite: [python.org](https://www.python.org/downloads/).

Nach der Installation könnt ihr im Terminal prüfen, ob Python gefunden wird:

```bash
python --version
```

Je nach Betriebssystem kann der Befehl auch `python3 --version` lauten. Entscheidend ist, dass eine Python-Version **3.14.x** angezeigt wird.

### Visual Studio Code

Als Editor verwenden wir im Kurs **Visual Studio Code (VS Code)**. VS Code kann kostenlos von [code.visualstudio.com](https://code.visualstudio.com/) installiert werden. Zusätzlich benötigen wir die offizielle **Python-Erweiterung** von Microsoft.

Damit VS Code weiß, welches Python es verwenden soll, kann über die Command Palette (`Ctrl+Shift+P` bzw. `Cmd+Shift+P`) der Befehl **Python: Select Interpreter** aufgerufen werden. Später wählen wir dort in der Regel die Python-Version aus dem jeweiligen Projekt-Environment (`.venv`) aus.

### `uv` und virtuelle Environments

Python-Projekte sollen ihre benötigten Bibliotheken möglichst nicht alle in eine einzige globale Python-Installation schreiben. Dafür nutzen wir **virtuelle Environments**. Im Kurs verwenden wir dafür `uv`, ein schnelles Werkzeug zum Verwalten von Python-Versionen, Environments und Paketen.

Die Installationsanleitung für `uv` findet ihr in der [offiziellen uv-Dokumentation](https://docs.astral.sh/uv/getting-started/installation/). Danach sollte

```bash
uv --version
```

eine Versionsnummer ausgeben.

Ein Environment im aktuellen Projektordner erstellen wir z.B. so:

```bash
uv venv --python 3.14
```

Standardmäßig entsteht dabei der Ordner `.venv`. Aktivieren lässt sich das Environment mit:

```bash
# macOS / Linux
source .venv/bin/activate
```

oder unter Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

VS Code erkennt ein lokales `.venv` normalerweise automatisch. Falls nicht, wählen wir es über **Python: Select Interpreter** aus.

### Wie wird Python ausgeführt?

Python-Code können wir auf verschiedene Arten ausführen. Die beiden wichtigsten Varianten in diesem Kurs sind das Terminal und VS Code.

### Python in der Kommandozeile

Eine einfache Möglichkeit ist der interaktive Python-Modus im **Terminal**. Gebt dazu `python` (oder je nach System `python3`) ein. Ihr solltet eine Python-Eingabeaufforderung sehen, die ungefähr so aussieht:

```
Python 3.14.x (...)
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Hier könnt ihr einzelne Python-Befehle direkt eingeben und ausführen:

```
>>> print("Hallo, Welt!")
Hallo, Welt!
```

### Python in VS Code

Den meisten Programmcode werden wir im **Editor** von VS Code schreiben und als `.py`-Dateien speichern. Eine geöffnete Python-Datei kann über **Run Python File** ausgeführt werden; die Ausgabe erscheint im integrierten Terminal.

VS Code bietet außerdem Syntax-Highlighting, Code-Vervollständigung, Hinweise auf mögliche Fehler und einen Debugger. Wichtig ist dabei immer, dass für das Projekt der richtige Python-Interpreter ausgewählt ist.

## Fehlermeldungen gehören dazu

Beim Programmieren werden ständig Fehlermeldungen auftreten. Das ist normal und ein wichtiger Teil des Lernens. Besonders hilfreich ist meist die **letzte Zeile** einer Fehlermeldung: Dort stehen der Fehlertyp und eine kurze Beschreibung.

Ein paar typische Beispiele:

<!-- pytest-codeblocks:expect-error -->
```python
print(dies)  # => NameError: name 'dies' is not defined
```

<!-- pytest-codeblocks:expect-error -->
```python
else = 5  # => SyntaxError
```

<!-- pytest-codeblocks:expect-error -->
```python
"5" + 2  # => TypeError
```

<!-- pytest-codeblocks:expect-error -->
```python
int("five")  # => ValueError
```

<!-- pytest-codeblocks:expect-error -->
```python
5 / 0  # => ZeroDivisionError
```

Später kommen weitere Fehlertypen dazu, z.B. `FileNotFoundError`, wenn eine angeforderte Datei nicht gefunden wird. Wichtig ist die Unterscheidung: Ein **Syntaxfehler** verhindert bereits das Starten des Programms; andere Fehler treten erst während der Ausführung auf. Und nicht jeder Programmfehler erzeugt überhaupt eine Fehlermeldung: Ein Programm kann auch ohne Exception laufen und trotzdem ein falsches Ergebnis berechnen.

## Kommentare in Python

Beim Programmieren ist es oft hilfreich, Kommentare in den Code einzufügen, um bestimmte Abschnitte zu erklären oder Notizen für sich selbst oder andere Entwickler*innen zu hinterlassen. Kommentare werden vom Python-Interpreter ignoriert und haben keinen Einfluss auf die Ausführung des Programms. Für normale Kommentare verwenden wir das Zeichen `#`:

**Einzeilige Kommentare**

Für einzeilige Kommentare wird das Rautezeichen `#` verwendet. Alles, was nach dem `#` steht, wird als Kommentar behandelt und nicht ausgeführt. Hier ein Beispiel:
```python
# Dies ist ein Kommentar
print("Hallo, Welt!")  # Dies ist ein weiterer Kommentar
```

Im obigen Beispiel wird die Zeile mit dem `print`-Befehl ausgeführt, während der Kommentar ignoriert wird.

**Mehrzeilige Kommentare**

Eine eigene Syntax für mehrzeilige Kommentare gibt es in Python nicht. Wenn mehrere Zeilen kommentiert werden sollen, setzt man normalerweise vor jede Zeile ein `#`:

```python
# Dieser Kommentar geht über
# mehrere Zeilen.
print("Hallo, Welt!")
```

Dreifache Anführungszeichen (`"""` oder `'''`) erzeugen dagegen einen **mehrzeiligen String**. An bestimmten Stellen, z.B. direkt am Anfang einer Funktion oder Klasse, wird ein solcher String als **Docstring** zur Dokumentation verwendet. Er ist also nicht einfach eine zweite Kommentar-Syntax.

## Ausführen von Python Code

Nachdem Python auf eurem Rechner installiert ist, gibt es verschiedene Wege, euren Python-Code auszuführen. Je nach Anwendungsfall könnt ihr z.B. das **Terminal** oder **Visual Studio Code** verwenden.

### Ausführen von Python-Skripten über die Kommandozeile

Wenn ihr ein Python-Skript geschrieben habt und es außerhalb einer IDE ausführen möchtet, könnt ihr dies ganz einfach über die **Kommandozeile** (Terminal bei Mac/Linux) tun. Ein typisches Szenario ist, dass ihr ein Skript namens `my_script.py` erstellt habt, das ihr nun ausführen wollt.

So geht's:

1. **Navigiert** im Terminal/Kommandozeile zu dem Verzeichnis, in dem euer Skript gespeichert ist.

   Unter Windows könnt ihr den Befehl `cd`  (Change Directory) verwenden, um zum entsprechenden Ordner zu wechseln:

   ```bash 
   cd Ordner1/Ordner2
   ```

   Auf Mac/Linux funktioniert dies ähnlich:

   ```bash
   cd /Ordner1/Ordner2
   ```

2. **Führt euer Skript aus**, indem ihr `python` (oder je nach System `python3`) gefolgt vom Namen eures Skripts eingebt:

   ```bash
   python my_script.py
   ```

   Dadurch wird der Python-Interpreter gestartet, der euer Skript Zeile für Zeile ausführt. Wenn euer Skript beispielsweise eine einfache Ausgabe wie `print("Hallo, Welt!")` enthält, wird diese direkt in der Kommandozeile angezeigt.

### Vorteile der Ausführung über die Kommandozeile

- **Direkte Kontrolle**: Ihr habt die volle Kontrolle darüber, wie euer Skript gestartet wird und könnt auch verschiedene Python-Versionen explizit verwenden, falls mehrere installiert sind (z. B. `python3 my_script.py` für Python 3).
- **Einfache Automatisierung**: Skripte lassen sich leicht in andere Tools integrieren oder automatisieren, etwa durch Batch-Dateien oder Shell-Skripte.
- **Leichtgewichtig**: Kein zusätzliches grafisches Interface nötig; ideal für schnelle Tests oder das Ausführen von Python-Programmen auf Servern.
