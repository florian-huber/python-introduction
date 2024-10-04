# Erste Schritte mit Python

In dieser Veranstaltungen wird Programmieren gelernt und zwar mit der Programmiersprache Python. Python wir euch aber auch das restliche DAISY-Studium über begleiten, denn wir arbeiten in sehr vielen Lehrveranstaltungen und Projekten damit. Um auch zuhause mit Python loslegen zu können, muss erstmal Python auf dem eigenen Rechner installiert werden. Und hier gibt es verschiedene Möglichkeiten: In vielen Fällen ist Python sogar schon auf dem Rechner vorinstalliert (z.B. bei Macs).

Falls Python nicht vorinstalliert ist, kann es über die offizielle Website heruntergeladen werden: [python.org](https://www.python.org/). Allerdings benötigen wir für diesen Kurs mehr als nur die Programmiersprache selbst, weshalb wir eine andere, umfassendere Lösung empfehlen:

## Python installieren

### Python und Anaconda

Die einfachste und bequemste Methode, um mit Python zu arbeiten, ist die Installation von **Anaconda**, einem Softwarepaket, das für Studierende kostenlos ist. Anaconda enthält neben Python auch viele nützliche Bibliotheken und Tools, die speziell für Data Science und Künstliche Intelligenz entwickelt wurden. Ein solches Tool ist die integrierte Entwicklungsumgebung (IDE) **Spyder**, die das Schreiben und Ausführen von Python-Code erleichtert.

Anaconda könnt ihr hier herunterladen: [Anaconda download link](https://www.anaconda.com/download/success)

Wählt die "Free Download"-Version aus, wobei keine E-Mail-Registrierung notwendig ist (einfach auf "skip registration" klicken). Achtet bei der Installation unter **Windows** darauf, das Häkchen bei der Frage, ob Anaconda zu eurem `PATH` hinzugefügt werden soll, zu setzen. Dies ist wichtig, damit Python in der Kommandozeile erkannt wird.

### Alternative: Miniconda

Für diejenigen, die sich bereits gut mit ihrem Computer auskennen oder eine minimale Installation bevorzugen, empfehlen wir die Installation von [**Miniconda**](https://docs.anaconda.com/miniconda/). Miniconda installiert nur die nötigsten Komponenten, und ihr könnt später individuell entscheiden, welche zusätzlichen Bibliotheken ihr benötigt. Dies erfordert jedoch ein wenig mehr Konfigurationsarbeit.


### Wie wird Python ausgeführt?

Sobald Python (über Anaconda oder Miniconda) installiert ist, gibt es verschiedene Möglichkeiten, Python-Code auszuführen. Die wichtigsten Methoden, die wir im Kurs verwenden werden, sind die Kommandozeile und die integrierte Entwicklungsumgebung (IDE) Spyder.

### Python in der Kommandozeile

Eine einfache Möglichkeit, Python auszuführen, ist über die **Kommandozeile** (oder das Terminal auf Mac/Linux). Um dies zu tun, öffnet ihr die Kommandozeile und gebt den Befehl `python` oder `python3` ein, je nachdem, wie Python auf eurem System eingerichtet ist. Ihr solltet nun eine Python-Eingabeaufforderung sehen, die etwa so aussieht:

```
Python 3.x.x (default, ...)
[GCC ...] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Hier könnt ihr einfache Python-Befehle direkt eingeben und sofort ausführen. Zum Beispiel:
```
>>> print("Hallo, Welt!")
```

### Python in der IDE Spyder

Eine komfortablere Möglichkeit, Python zu nutzen, bietet die integrierte Entwicklungsumgebung (IDE für "Integrated Development Environment") **Spyder**, die mit Anaconda geliefert wird aber auch frei davon als Open-Source Software verfügbar ist. Spyder bietet eine grafische Benutzeroberfläche, in der ihr euren Code schreiben, ausführen und die Ergebnisse sehen könnt. Hier ist der Arbeitsablauf in Spyder:

1. **Editor:** Im Hauptfenster von Spyder schreibt ihr euren Python-Code im Editor. Hier könnt ihr auch größere Programme oder Skripte schreiben, die aus mehreren Zeilen Code bestehen.
2. **Ausführen:** Um euren Code auszuführen, klickt ihr auf das grüne "Play"-Symbol oben im Fenster oder drückt die Tastenkombination `F5`. Der Code wird ausgeführt, und die Ergebnisse erscheinen in der **Konsole**.
3. **Variablen-Explorer:** Ein weiterer Vorteil von Spyder ist der **Variablen-Explorer**, der es euch ermöglicht, alle aktuell im Programm verwendeten Variablen zu sehen. Dies ist besonders nützlich, um den Überblick über größere Datenmengen zu behalten, was im Bereich Data Science oft der Fall ist.

Spyder ist eine leistungsfähige Umgebung für das Schreiben und Debuggen von Python-Code. Es bietet viele Funktionen, die euch helfen, effizienter zu arbeiten, wie z.B. Code-Vervollständigung, Syntax-Highlighting und integrierte Debugging-Tools.
Im Laufe des Studiums werden viele auch zu anderen Editoren greifen, allem voran **Visual Studio Code** das einen noch großeren Funktionsumfang bietet. Insbesondere für Anfänger*innen ist Spyder aber oft übersichtlicher weshalb wir im Kurs in diesem Semester damit arbeiten werden.

Den meisten Programmcode werden wir hier im **Editor** erstellen. Die erlaubt es, umfangreicheren Code über viele Zeilen zu schreiben und als `.py`-Dateien zu speichern. Spyder, wie die meisten IDEs für Python gibt dabei auch regelmäßig hinweise auf mögliche Fehler oder andere Hilfestellungen. Wichtig an dieser Stelle ist zudem, dass wir eben nicht nur Code sondern auch Kommentare erstellen können.

## Kommentare in Python

Beim Programmieren ist es oft hilfreich, Kommentare in den Code einzufügen, um bestimmte Abschnitte zu erklären oder Notizen für sich selbst oder andere Entwickler*innen zu hinterlassen. Kommentare werden vom Python-Interpreter ignoriert und haben keinen Einfluss auf die Ausführung des Programms. Es gibt zwei Möglichkeiten, Kommentare in Python hinzuzufügen:

**Einzeilige Kommentare**

Für einzeilige Kommentare wird das Rautezeichen `#` verwendet. Alles, was nach dem `#` steht, wird als Kommentar behandelt und nicht ausgeführt. Hier ein Beispiel:
```python
# Dies ist ein Kommentar
print("Hallo, Welt!")  # Dies ist ein weiterer Kommentar
```

Im obigen Beispiel wird die Zeile mit dem `print`-Befehl ausgeführt, während der Kommentar ignoriert wird.

**Mehrzeilige Kommentare**

Für **mehrzeilige Kommentare** gibt es keine spezielle Syntax in Python, aber man kann dafür dreifache Anführungszeichen (`"""` oder `'''`) verwenden, um einen sogenannten **Docstring** zu erstellen. Obwohl Docstrings in erster Linie zur Dokumentation von Funktionen oder Klassen verwendet werden, können sie auch genutzt werden, um längere Kommentare im Code einzufügen:

```python
"""
Dies ist ein mehrzeiliger Kommentar.
Er kann verwendet werden, um ausführliche Erklärungen oder Dokumentation hinzuzufügen.
"""
print("Hallo, Welt!")
```

## Ausführen von Python Code

Nachdem Python auf eurem Rechner installiert ist, gibt es verschiedene Wege, euren Python-Code auszuführen. Je nach Anwendungsfall könnt ihr zwischen der **Kommandozeile**, einer integrierten Entwicklungsumgebung (IDE) wie **Spyder**, oder speziellen Editoren wie **Visual Studio Code** wählen.

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

2. **Führt euer Skript aus**, indem ihr den Befehl `python` oder `python3` (je nach eurer Installation) gefolgt vom Namen eures Skripts eingebt:

   ```bash
   python my_script.py
   ```

   Dadurch wird der Python-Interpreter gestartet, der euer Skript Zeile für Zeile ausführt. Wenn euer Skript beispielsweise eine einfache Ausgabe wie `print("Hallo, Welt!")` enthält, wird diese direkt in der Kommandozeile angezeigt.

### Vorteile der Ausführung über die Kommandozeile

- **Direkte Kontrolle**: Ihr habt die volle Kontrolle darüber, wie euer Skript gestartet wird und könnt auch verschiedene Python-Versionen explizit verwenden, falls mehrere installiert sind (z. B. `python3 my_script.py` für Python 3).
- **Einfache Automatisierung**: Skripte lassen sich leicht in andere Tools integrieren oder automatisieren, etwa durch Batch-Dateien oder Shell-Skripte.
- **Leichtgewichtig**: Kein zusätzliches grafisches Interface nötig; ideal für schnelle Tests oder das Ausführen von Python-Programmen auf Servern.