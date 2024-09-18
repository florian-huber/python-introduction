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