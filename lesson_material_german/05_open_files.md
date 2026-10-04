# Dateien lesen und schreiben

Bei Dateien unterscheidet man grundlegend zwischen Binärdateien und Textdateien. Binärdateien bestehen aus Bitmustern und erlauben es Daten effizient zu hinterlegen. Sie finden in vielen bekannten Datenformaten Verwendung, z.B. vielen Bild-, Video- oder Audio-Dateien. In der Regel sind solche Dateien aber nur mit passenden Tools (oder Python Bibliotheken) importierbar. Häufig werden Daten aber auch in [Textdateien](https://de.wikipedia.org/wiki/Textdatei) gespeichert. Dies beinhaltet nicht nur die `.txt` Dateien oder Dateien mit "Text", sondern im Prinzip alle Dateien die mit einem normalen Texteditor lesbar sind (also genau das Gegenstück zu den Binärdateien für die dies nicht gilt).

Beispiele die uns im Laufe der Veranstaltung noch häufiger begegnen werden sind .csv, .yml oder .yaml. Aber auch die .py Dateien gehören zu den Textdateien.



Dateien lassen sich in Python mit `open()` lesen und erstellen. Im Folgenden - sowie in der Praxis fast immer- werden wir uns nur mit Textdateien beschäftigen. 

Hier ein Beispiel für eine Datei `testfile.txt` die sich im selben Ordner befindet:
<!--pytest-codeblocks:skip-->

```python 
data = open("testfile.txt", "r", encoding="utf-8")
print(data.readline())  # => 'Hier mal ein wenig Text zum Testen.\n'
data.close()
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
|     x       |     exklusives Erstellen, Textmodus  |     Erstellt eine neue Datei zum Schreiben. Falls die Datei bereits existiert, wird ein `FileExistsError` ausgelöst.                                                                                             |
|     xb      |     exklusives Erstellen, Binärmodus |                                                                                                                                                                                                                 |


Es ist auch einfach möglich die Datei Zeile für Zeile auszulesen.
<!--pytest-codeblocks:skip-->
```python 
data = open("testfile.txt", "r", encoding="utf-8")
for line in data:
    print(line)
data.close()
```
Sollte die Datei sich nicht im selben Ordner befinden, muss der Pfad noch 
zum Dateinamen hinzugefügt werden.

+ Pfad unter Windows: "C:/User/my_folder/testfile.txt" oder jeweils mit \\\\
+ Pfad unter MacOS und Linux: "/home/user/my_folder/testfile.txt"

Es ist auch möglich dies über *relative* Pfadangaben zu machen, z.B.

- `data = open("my_data/testfile2.txt", "r")` oder:
  `data = open("my_data\\testfile2.txt", "r")`

Eine weitere Möglichkeit (dazu kommen wir später noch ausführlicher), ist die Nutzung von Python-Modulen zum Pfad-Handling, z.B. `pathlib` oder `os`.

### Write
Das Schreiben von Dateien geschieht über `write()`. Aber auch hier wird
zuerst eine Datei geöffnet um anschließend in diese Datei zu schreiben.

```python 
output = open("output_file.txt", "w")
text = "Kommen wir nun zu etwas völlig anderem."
output.write(text + "\n")
output.close()
```
Während im mode "w" (write) jedes Mal eine neue Datei erstellt wird, können
wir mit "a" auch eine vorhandene Datei weiter schreiben.
```python 
output = open("output_file.txt", "a")
output.write("So. Ja. Genau.\n")
output.write("Ja " * 5 + "\n")
output.close()
```
Im Alltag ist `with` meist bequemer und sicherer als ein manuelles `open()`/`close()`, weil die Datei am Ende des Blocks automatisch geschlossen wird:

```python 
with open("output_file.txt", "w") as file:
    for i in range(1, 6):
        file.write(f"Zeile {i} \n")
```
```python 
with open("output_file.txt", "r") as file:
    for line in file:
        if "3" in line:
            print(line)
```
## Zeichenkodierung
Computer kennen erstmal keine Buchstaben oder Zeichen, sondern nur Bytes. Bytes sind Zahlen im Bereich von 0 bis 255. Um mit Zahlen einen Text darstellen zu können, braucht der Computer eine Zuordnungstabelle, in der steht, welche Zahl für welchen Buchstaben stehen soll. 

Es gibt zahlreiche verschiedene Kodierungen, die häufigsten sind aber:
### ASCII
ASCII steht für *American Standard Code for Information Interchange* und definiert 128 Zeichen (0 bis 127), darunter die englischen Buchstaben, Ziffern und einige Steuerzeichen. Umlaute oder viele andere Schriftsysteme sind darin nicht enthalten.

### Unicode
Unicode definiert einen sehr großen gemeinsamen Zeichenvorrat für Schriftsysteme und Symbole aus aller Welt. **UTF-8** ist eine weit verbreitete Kodierung, mit der Unicode-Zeichen als Bytes gespeichert werden. UTF-8 ist heute für Textdateien und Web-Inhalte ein sehr häufiger Standard.

Für mehr Informationen zu diesem Thema: https://wiki.selfhtml.org/wiki/Zeichencodierung

Sollten einmal Probleme mit Umlauten oder anderen Sonderzeichen auftreten, wurde eine Textdatei häufig mit einer anderen Kodierung gelesen oder geschrieben als erwartet.

Um sicher zu gehen, kann die Kodierung mit angegeben werden:
```python 
with open("output_file.txt", "w", encoding="utf-8") as file:
    for i in range(1, 6):
        file.write(f"Zeile {i} \n")
```

