# Dateien lesen und schreiben

Häufig werden Daten in [Textdateien](https://de.wikipedia.org/wiki/Textdatei)
gespeichert. Dies beinhaltet nicht nur
die `.txt` Dateien sondern im Prinzip alle Dateien die mit einem normalen
Texteditor lesbar sind (das Gegenstück dazu sind Binärdateien für die dies
nicht gilt).

Beispiele die uns im Laufe der Veranstaltung noch häufiger begegnen werden sind
.csv, .yml oder .yaml. Aber auch die .py Dateien gehören zu den Textdateien.



Eine Textdatei kann man in Python mit `open()` lesen. Hier ein Beispiel
für eine Datei `testfile.txt` die sich im selben Ordner befindet:
```python 
data = open("testfile.txt", "r")
data.readline()  # => 'Hier mal ein wenig Text zum Testen.\n'
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
```python 
data = open("testfile.txt", "r")
for line in data:
    print(line)
```
Sollte die Datei sich nicht im selben Ordner befinden, muss der Pfad noch 
zum Dateinamen hinzigefügt werden.
+ Pfad unter Windows: "C:/User/my_folder/testfile.txt" oder mit //
+ Pfad unter MacOS und Linux: "/home/user/my_folder/testfile.txt"

### Write
Das Schreiben von Dateien geschieht über `write()`. Aber auch hier wird
zuerst eine Datei geöffnet um anschliessend in diese Datei zu schreiben.
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
Codecs
Text....
open(encoding="utf-8")

import os

print(os.getcwd())


