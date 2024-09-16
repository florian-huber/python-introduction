# Dateien lesen und schreiben

Bei Dateien unterscheidet man grundlegend zwischen Binärdateien und Textdateien. Binärdateien bestehen aus Bitmustern und erlauben es Daten effizient zu hinterlegen. Sie finden in vielen bekannten Datenformaten Verwendung, z.B. vielen Bild-, Video- oder Audio-Dateien. In der Regel sind solche Dateien aber nur mit passenden Tools (oder Python Bibliotheken) importierbar. Häufig werden Daten aber auch in [Textdateien](https://de.wikipedia.org/wiki/Textdatei) gespeichert. Dies beinhaltet nicht nur die `.txt` Dateien oder Dateien mit "Text", sondern im Prinzip alle Dateien die mit einem normalen Texteditor lesbar sind (also genau das Gegenstück zu den Binärdateien für die dies nicht gilt).

Beispiele die uns im Laufe der Veranstaltung noch häufiger begegnen werden sind .csv, .yml oder .yaml. Aber auch die .py Dateien gehören zu den Textdateien.



Dateien lassen sich in Python mit `open()` lesen und erstellen. Im Folgenden - sowie in der Praxis fast immer- werden wir uns nur mit Textdateien beschäftigen. 

Hier ein Beispiel für eine Datei `testfile.txt` die sich im selben Ordner befindet:
<!--pytest-codeblocks:skip-->

```python 
data = open("testfile.txt", "r")
print(data.readline())  # => 'Hier mal ein wenig Text zum Testen.\n'
```
Neben dem Dateinamen wird hier auch `"r"` angegeben, was für den Modus der
Funktion steht. Die wichtigsten Varianten davon finden sich in der Tabelle:


|     mode    |     Bedeutung                         |     Erklärung                                                                                                                                                                                                   |
|-------------|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     r       |     read -> Lesen, Textmodus          |     Dateicursor   am Anfang der Datei. Neue Daten werden nicht angelegt.                                                                                                                                        |
|     rb      |     read -> Lesen, Binärmodus         |                                                                                                                                                                                                                 |
|     w       |     write -> Schreiben, Textmodus     |     Dateicursor   am Anfang der Datei. Falls eine Datei mit dem gleichen Namen schon existiert,   wird deren Inhalt gelöscht und neu beschrieben. Andernfalls wird eine neue   Datei zum Schreiben angelegt.    |
|     wb      |     write -> Schreiben, Binärmodus    |                                                                                                                                                                                                                 |
|     a       |     Anhängen, Textmodus               |     Dateicursor am Ende der Datei. Der bisherige Inhalt wird nicht   gelöscht, es wird am Ende weiter geschrieben.                                                                                              |
|     ab      |     Anhängen, Binärmodus              |                                                                                                                                                                                                                 |
|     x       |                                       |     Gleich wie r (rb),   aber existierende Dateien werden nicht überschrieben.                                                                                                                                  |
|     xb      |                                       |                                                                                                                                                                                                                 |


Es ist auch einfach möglich die Datei Zeile für Zeile auszulesen.
<!--pytest-codeblocks:skip-->
```python 
data = open("testfile.txt", "r")
for line in data:
    print(line)
```
Sollte die Datei sich nicht im selben Ordner befinden, muss der Pfad noch 
zum Dateinamen hinzugefügt werden.

+ Pfad unter Windows: "C:/User/my_folder/testfile.txt" oder jeweils mit \\\\
+ Pfad unter MacOS und Linux: "/home/user/my_folder/testfile.txt"

Es ist auch möglich dies über *relative* Pfadangaben zu machen, z.B.

- `data = open("my_data/testfile2.txt", "r")` oder:
  `data = open("my_data\\testfile2.txt", "r")`

Eine weitere Möglichkeit (dazu kommen wir später noch ausführlicher), ist die Nutzung von Python-Bibliotheken zum Pfad-Handling, z.B. `os`.

### Write
Das Schreiben von Dateien geschieht über `write()`. Aber auch hier wird
zuerst eine Datei geöffnet um anschließend in diese Datei zu schreiben.

```python 
output = open("ouput_file.txt", "w")
text = "Kommen wir nun zu etwas völlig anderem."
output.write(text + "\n")
output.close()
```
Während im mode "w" (write) jedes Mal eine neue Datei erstellt wird, können
wir mit "a" auch eine vorhandene Datei weiter schreiben.
```python 
output = open("ouput_file.txt", "a")
output.write("So. Ja. Genau.\n")
output.write("Ja " * 5 + "\n")
output.close()
```
Im Alltag häufig einfach als open/close ist das verwenden von `with`:

```python 
with open("ouput_file.txt", "w") as file:
    for i in range(1, 6):
        file.write(f"Zeile {i} \n")
```
```python 
with open("ouput_file.txt", "r") as file:
    for line in file:
        if "3" in line:
            print(line)
```
## Zeichenkodierung
Computer kennen erstmal keine Buchstaben oder Zeichen, sondern nur Bytes. Bytes sind Zahlen im Bereich von 0 bis 255. Um mit Zahlen einen Text darstellen zu können, braucht der Computer eine Zuordnungstabelle, in der steht, welche Zahl für welchen Buchstaben stehen soll. 

Es gibt zahlreiche verschiedene Kodierungen, die häufigsten sind aber:
### ASCII
ASCII steht für *American Standard Code for Information Interchange* und kann 256 verschiedene Zeichen darstellen. Die meisten Sonderzeichen sind daher nicht enthalten.

### Unicode
Unicode ist mittlerweile der Standard, v.a. im Internet (HTML 4.0) und enthält mehr als 100.000 Zeichen.
Es gibt verschiedene Unicode Kodierungen, am meisten genutzt in Python ist allerdings **utf-8** da dies relativ elegant (=Speicher sparend) die Zeichen kodieren kann.

Für mehr Informationen zu diesem Thema: https://wiki.selfhtml.org/wiki/Zeichencodierung

Sollten einmal Probleme mit Umlauten/Sonderzeichen auftreten, liegt dies in der Regal daran, dass bei einer Textdatei nicht richtig zwischen ASCII und utf-8 konvertiert wurde.

Um sicher zu gehen, kann de Kodierung mit angegeben werden:
```python 
with open("ouput_file.txt", "w", encoding="utf-8") as file:
    for i in range(1, 6):
        file.write(f"Zeile {i} \n")
```

