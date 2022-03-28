# Python Debugging 

### Ein wenig wie Hieroglyphen lesen...

Inzwischen haben Sie schon eine Menge erste Programmiererfahrung mit Python sammeln können. Und sicher kennen Sie darum inzwischen auch die Situation, dass sie einen bestimmten Code zum Laufen bringen wollen, aber ständig nur Error-Nachrichten zurück bekommen! Und trotz allem hin- und her probieren will der Code einfach nicht so laufen wie gewünscht.

Die schlechte Nachricht zuerst: das bleibt so. Auch mit mehr Programmiererfahrung. :(

Die gute Nachricht ist aber, dass die Fehlermeldung nach und nach anfangen etwas mehr Sinn zu machen, so dass auch oft schneller klar wird wo der eigentliche Fehler liegt.

Aber fangen wir einfach an:

```python
primt("hello")
```

 gibt

```
NameError: name 'primt' is not defined
```

Das passiert ständig: Tippfehler. `primt` gibt es nicht, wir wollten natürlich auch `print` schreiben. Zack, gelöst. Einfach.

## Mehr Code, mehr Text, mehr Fehlerquellen

So ein Einzeiler wie oben ist natürlich auch deshalb so leicht zu lösen, weil der Fehler ja nur an wenigen Stellen entstanden sein kann. In der Regel arbeiten wir aber jetzt schon mit deutlich komplizierterem Code, mit selbst geschriebenen Funktionen und Klassen, mit importierten Bibliotheken und und und...

Und hier wird es schnell unübersichtlich, und zwar aus verschiedenen Gründen.

### Jedes Zeichen zählt... beim Computer aber NICHT beim Menschen!

Die menschliche, visuelle Wahrnehmung ist gut um schnell das grobe Ganze zu erfassen. Aber eben nicht immer auch so gut darin, kleine Abweichungen zu erkennen. Wahrscheinlich können sie darum den folgenden Text relativ problemlos lesen:

*Wir könenn als Mneschen schenll den Ihalnt von Teetxn vrestheen und schenll veile Wötrer leesn. Aebr wir snid nchit bdnrseoes gut im Etneckden von keilenn Fehelrn wie zum Biespiel vretaueschtn Bchustbaen oder feehlnedn Stazzechein.*

Dieses Phänomen ist in der Forschung gut bekannt, siehe [wikipedia](https://en.wikipedia.org/wiki/Transposed_letter_effect
) oder [hier](https://www.mrc-cbu.cam.ac.uk/people/matt.davis/cmabridge/
). Verkürzt bedeutet es, dass wir eben nicht so gut darin sind kleine Buchstabendreher zu entdecken. Programmiersprachen sind da aber ein wenig anders. Darum scheitern wir ständig an unseren kleinen Tippfehlern...

```python
class SingleAttribute:
    supercomplicatedattributename = 3.14245
    
thisisanobjectofaclassthatdoesnearlynothing = SingleAttribute()
```

Diese Attribut und Objektnamen hier wurden nicht besonders gut gewählt. Schon gar nicht wenn wir an unsere eingeschränkte Fähigkeit denken, kleine Wort Änderungen zu bemerken.

Im folgenden sehen wir einige Fehler die entstehen können:

```python
print(thisisanobjectofaclassthatdoesnearlynothing.supercomplicatedattributename())

TypeError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/3080376188.py in <module>
----> 1 print(thisisanobjectofaclassthatdoesnearlynothing.supercomplicatedattributename())

TypeError: 'float' object is not callable
```

So sieht es zumindest in einem Jupyter Notebook aus. Irgendwas mit einem `TypeError` also.

Wenn wir die Fehlermeldung genauer lesen steht da `'float' object is not callable`  und das entscheidende Stichwort in diesen Hieroglyphen ist `callable` (also "aufrufbar"). Aufrufbar sind in Python die Funktionen und Methoden und zwar immer mit `my_function_soundso()`, also den runden Klammern.
Diese sind in dem Beispiel hier auch genau das Problem, den ich probiere hier ein Attribut (also eine Klassen-Variable) als eine Methode oder Funktion aufzurufen, was natürlich nicht geht.

```python
print(thisisanobjectofaclassthatdoesnearlynothing:supercomplicatedattributename)

  File "C:\Users\xyz\AppData\Local\Temp/ipykernel_15368/2591865100.py", line 1
    print(thisisanobjectofaclassthatdoesnearlynothing:supercomplicatedattributename)
                                                     ^
SyntaxError: invalid syntax
```

Hier ist Python wirklich nett und hilfreich. Es geht um einen `SyntaxError`, das heißt irgendwas wurde hier wohl nicht richtig getippt, vielleicht eine Klammer, ein Anführungszeichen zu viel oder zu wenig?
Wir müssen nicht mal selber suchen, denn der Pfeil (`^`) gibt genau an, wo etwas nicht stimmt. Tja, Doppelpunkt statt Punkt... kann passieren.

```python
print(thisisanobjectofaclassthatdoesnearlynothing.supercomplicatedatttributename)

AttributeError                            Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/4100655615.py in <module>
----> 1 print(thisisanobjectofaclassthatdoesnearlynothing.supercomplicatedatttributename)

AttributeError: 'SingleAttribute' object has no attribute 'supercomplicatedatttributename'
```

Das ist eigentlich auch recht deutlich, solange wir uns auf die letzte Zeile konzentieren. Wir haben einen `AttributeError`, also irgendwas mit einem Attribut des Objekts stimmt nicht. Und es wird sogar genannt welches Attribut, nämlich `supercomplicatedatttributename`was in diesem Fall natürlich auch das einzige Attribut ist, das in unserem Code vorkommt. Da Python sagt, dass das Objekt dieses Attribut nicht hat (`object has no attribute 'supercomplicatedatttributename'`) wäre die erste Vermutung, dass wir es vielleicht nicht richtig geschrieben haben. Das stimmt übrigens auch, ein t zuviel im Name.

Also Zusammenfassung:

- Alle Zeichen, sogar Groß-Kleinschreibung zählen
- Menschen haben dafür aber keine besonders gute Wahrnehmung



### Oft wird der Fehler nicht dort entdeckt wo er gemacht wird.

- Funktionen, Bibliotheken, Klassen… sind häufig sehr verschachtelt.
- Häufig bekommen wir sehr lange Fehlermeldungen zurück die, die auf sehr tief verschachtelte Funktionen oder Klassen verweisen in denen der Fehler von Python eine Ausnahme (exception) auslöst.
- **Extra** **schwierig:** Dadurch werden oft auch Fehler benannt, die uns auf ein falsche Fährte führen.

```python
def function1(number):
    if number % 2 == 0:
        print("Hurray, eine gerade Zahl!")
    else:
        print(number)

        
def function2(number):
    number = number + number
    function1(number)

    
def function3(number):
    number = function2(number)


function3("400")
```

Hier haben wir drei Funktionen die verschachtelt sind. `function3`ruft `function2` auf und diese wiederum ruft `function1` auf.

Die Fehlermeldung in diesem Fall wird schon deutlich komplizierter:

```
TypeError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/989526411.py in <module>
     13 
     14 
---> 15 function3("400")

~\AppData\Local\Temp/ipykernel_15368/989526411.py in function3(number)
     10 
     11 def function3(number):
---> 12     number = function2(number)
     13 
     14 

~\AppData\Local\Temp/ipykernel_15368/989526411.py in function2(number)
      7 def function2(number):
      8     number = number + number
----> 9     function1(number)
     10 
     11 def function3(number):

~\AppData\Local\Temp/ipykernel_15368/989526411.py in function1(number)
      1 def function1(number):
----> 2     if number % 2 == 0:
      3         print("Hurray, eine gerade Zahl!")
      4     else:
      5         print(number)

TypeError: not all arguments converted during string formatting
```

Hier ist die Suche etwas umständlicher. Aber der Reihe nach, und zwar gehen wir hier **von unten nach oben**!

1) Der Fehler wegen dem das Programm abstürzt ist ein `TypeError` und zwar irgendwas mit einem `string`.
2) Der Pfeil an der linken Seite zeigt auf die Zeile 2 bei `function1` in der steht `if number % 2 == 0:`
3) `function1` wurde aus `function2` aufgerufen, Zeile 9.
4) `function2` wurde aus `function3` aufgerufen, Zeile 12.
5) `function3("400")` in zeile 15 ist der Startpunkt für das Ganze.

Der eigentliche Fehler entsteht beim untersten Codeteil, also in der Zeile  `if number % 2 == 0:`
Und wir wissen, dass es ein TypeError hat, vermutlich hat also `number` nicht den passenden Typ für die dort ausgeführte Aktion. Macht auch Sinn, denn wir haben als number ja `"400"`, also einen String eingegeben!

Ein weitere Hilfe hier kann es übrigens sein, zusätzliche `print`-Statements einzubauen um genauer herauszufinden, was wann wo schief geht.

Ausserdem können wir unsere Hypothesen überprüfen, indem wir nur die kritische Stelle nachbauen:

```python
"400" % 2

TypeError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/4268186087.py in <module>
----> 1 "400" % 4

TypeError: not all arguments converted during string formatting
```

Damit sollte klar sein, wo der Fehler hier liegt.

Wir werden gleich noch mehr Varianten aus dem echten Leben sehen. Aber erst noch ein verwandtes Beispiel. Beim Benutzen importierter Funktionen können ebenfalls Fehler auftreten, die uns nicht direkt deutlich verraten was wir falsch gemacht haben. Zumindest nicht, wenn wir nicht genug über die entsprechenden Funktionen wissen.

```python
import numpy as np

rows = 6
cols = 9
arr = np.zeros(rows, cols)

TypeError                                 Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/827775635.py in <module>
      3 rows = 6
      4 cols = 9
----> 5 arr = np.zeros(rows, cols)

TypeError: Cannot interpret '9' as a data type
```

Das kann erstmal verwirren, denn Python sagt uns, dass es die 9 nicht als `data type` interpretieren kann. Soll es ja auch nicht!
Der Fehler hier liegt woanders. Nicht die 9 ist das Problem, sondern die Art wie wir die Arraygröße der Funktion übergeben haben. In einem solchen Fall, hilft es in der Regel sich die Funktion selber (bzw. deren Dokumentation) anzuschauen. In diesem Fall geht das einfach über die [Online-Dokumentation von Numpy ](https://numpy.org/doc/stable/reference/generated/numpy.zeros.html), aber wir können auch innerhalb unserer Programmierumgebung die Docstrings von importierten Funktionen ansehen mit `print(np.zeros.__doc__)`. Dort sehen wir dann, dass die Funktion `zeros` folgende Parameter als Eingabe erwartet: `zeros(shape, dtype=float, order='C')` also `shape` und optional auch noch `dtype` und weitere. Die Größe muss also als **eine** `shape` eingegeben werden. Dies seht auch noch ausführlicher in der Dokumentation:  

```
shape : int or tuple of ints
        Shape of the new array, e.g., ``(2, 3)`` or ``2``.
```

Es muss also heißen:

```python
import numpy as np

rows = 6
cols = 9
arr = np.zeros(shape=(rows, cols))

# oder:
arr = np.zeros((rows, cols))
```

## Ein Minenfeld: File import!

In den Übungen haben wir jetzt schon öfter die kleinen Schwierigkeiten beim Importieren von Dateien genossen. Zwei Fehler dürften dabei alle inzwischen einmal selber mitbekommen haben: `UnicodeDecodeError` und am beliebtesten: `FileNotFoundError`!

Fangen wir also an eine Datei zu importieren: "NetflixOriginals.csv"

```python
import pandas as pd

filename = "C:\Users\myname\NetflixOriginals.csv"  # durch eigenen Pfadname ersetzen
data = pd.read_csv(filename)

File "C:\Users\...\AppData\Local\Temp/ipykernel_15368/2604329180.py", line 1
    filename = "C:\Users\myname\NetflixOriginals.csv"
               ^
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 100-101: malformed \N character escape
```

OK. Was hier schief ging ist, dass wir auf einem Windows Rechner mit einfachen Schrägstrichen arbeiten `\`. Diese werden aber als Escape-Zeichen behandelt und verweisen auf spezielle Zeichen z.B. `\n` für Zeilenumbruch oder `\t` für Tab. Wir müssten hier (unter Windows) die Backslash Zeichen durch doppelte Backslash ersetzen --> `\\`

Da die Pfadstrukturen auf den verschiedenen Systemen (Windows, Linux, Mac) aber unterschiedlich funktionieren, sollten wir besser zu anderen Methoden greifen. Eine davon ist das benutzen der Python-Standardbibliothek `os`:

```python
import os

print(os.getcwd())  # => gibt den aktuellen Pfad aus
```

Mit der `os` Bibliothek können wir auf Elemente des "**o**perating **s**ystem" zugreifen. Wir können damit Pfadstrukturen einlesen und verändern. Das ist auch mit etwas vorsicht zu genießen, denn wir können mit os auch neue Ordner erstellen `os.mkdir(path)` aber auch Ordner löschen mit `os.remove(path)` oder `os.removedirs(path)`, sowie umbenennen: `os.rename()`.

Praktische Funktionen zum Umgang mit Pfaden finden sich im Teil `os.path`. 

```python
import os
path = os.getcwd()

os.path.basename(path)  # obersten Ordner
os.path.dirname(path)  # darunter liegender Pfad
os.path.exists(path)  # return True if path exists
```

Darum lässt sich dies auch gut für das Importieren von Dateien nutzen und der Code funktioniert dann (im Prinzip) auch auf anderen Systemen.

```python
root = os.getcwd()
filename = os.path.join(root, "exercises", 'NetflixOrginals.csv')  # je nach eigenem Pfad anpassen!

data = pd.read_csv(filename, delimiter=",")
```

Oh, dies gibt aber auch einen Fehler (und was für einen...):

```
FileNotFoundError                         Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_15368/1093288611.py in <module>
      2 filename = os.path.join(root, "exercises", 'NetflixOrginals.csv')  # je nach eigenem Pfad anpassen!
      3 
----> 4 data = pd.read_csv(filename, delimiter=",")

~\.conda\envs\python_introduction\lib\site-packages\pandas\util\_decorators.py in wrapper(*args, **kwargs)
    309                     stacklevel=stacklevel,
    310                 )
--> 311             return func(*args, **kwargs)
    312 
    313         return wrapper

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in read_csv(filepath_or_buffer, sep, delimiter, header, names, index_col, usecols, squeeze, prefix, mangle_dupe_cols, dtype, engine, converters, true_values, false_values, skipinitialspace, skiprows, skipfooter, nrows, na_values, keep_default_na, na_filter, verbose, skip_blank_lines, parse_dates, infer_datetime_format, keep_date_col, date_parser, dayfirst, cache_dates, iterator, chunksize, compression, thousands, decimal, lineterminator, quotechar, quoting, doublequote, escapechar, comment, encoding, encoding_errors, dialect, error_bad_lines, warn_bad_lines, on_bad_lines, delim_whitespace, low_memory, memory_map, float_precision, storage_options)
    584     kwds.update(kwds_defaults)
    585 
--> 586     return _read(filepath_or_buffer, kwds)
    587 
    588 

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in _read(filepath_or_buffer, kwds)
    480 
    481     # Create the parser.
--> 482     parser = TextFileReader(filepath_or_buffer, **kwds)
    483 
    484     if chunksize or iterator:

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in __init__(self, f, engine, **kwds)
    809             self.options["has_index_names"] = kwds["has_index_names"]
    810 
--> 811         self._engine = self._make_engine(self.engine)
    812 
    813     def close(self):

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in _make_engine(self, engine)
   1038             )
   1039         # error: Too many arguments for "ParserBase"
-> 1040         return mapping[engine](self.f, **self.options)  # type: ignore[call-arg]
   1041 
   1042     def _failover_to_python(self):

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\c_parser_wrapper.py in __init__(self, src, **kwds)
     49 
     50         # open handles
---> 51         self._open_handles(src, kwds)
     52         assert self.handles is not None
     53 

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\base_parser.py in _open_handles(self, src, kwds)
    220         Let the readers open IOHandles after they are done with their potential raises.
    221         """
--> 222         self.handles = get_handle(
    223             src,
    224             "r",

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\common.py in get_handle(path_or_buf, mode, encoding, compression, memory_map, is_text, errors, storage_options)
    700         if ioargs.encoding and "b" not in ioargs.mode:
    701             # Encoding
--> 702             handle = open(
    703                 handle,
    704                 ioargs.mode,

FileNotFoundError: [Errno 2] No such file or directory: 'C:\\User\\myname\\exercises\\NetflixOrginals.csv'
```

Hier kann getrost alles bis auf das Ende der Nachricht ignoriert werden! Die Datei `'C:\\User\\myname\\exercises\\NetflixOrginals.csv'` wird schlicht nicht gefunden. Und das alles nur wegen einem einzigen fehlenden "i" --> Orginals --> Or`i`ginals.

OK. Nachdem wir das hinbekommen haben müsste es gehen. Tut es natürlich aber nicht:

```python
root = os.getcwd()
filename = os.path.join(root, "exercises", 'NetflixOriginals.csv')  # je nach eigenem Pfad anpassen!

data = pd.read_csv(filename, delimiter=",")

UnicodeDecodeError                        Traceback (most recent call last)
~\AppData\Local\Temp/ipykernel_25048/1030038016.py in <module>
      5 filename = os.path.join(root, "exercises", 'NetflixOriginals.csv')  # je nach eigenem Pfad anpassen!
      6 
----> 7 data = pd.read_csv(filename, delimiter=",")

~\.conda\envs\python_introduction\lib\site-packages\pandas\util\_decorators.py in wrapper(*args, **kwargs)
    309                     stacklevel=stacklevel,
    310                 )
--> 311             return func(*args, **kwargs)
    312 
    313         return wrapper

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in read_csv(filepath_or_buffer, sep, delimiter, header, names, index_col, usecols, squeeze, prefix, mangle_dupe_cols, dtype, engine, converters, true_values, false_values, skipinitialspace, skiprows, skipfooter, nrows, na_values, keep_default_na, na_filter, verbose, skip_blank_lines, parse_dates, infer_datetime_format, keep_date_col, date_parser, dayfirst, cache_dates, iterator, chunksize, compression, thousands, decimal, lineterminator, quotechar, quoting, doublequote, escapechar, comment, encoding, encoding_errors, dialect, error_bad_lines, warn_bad_lines, on_bad_lines, delim_whitespace, low_memory, memory_map, float_precision, storage_options)
    584     kwds.update(kwds_defaults)
    585 
--> 586     return _read(filepath_or_buffer, kwds)
    587 
    588 

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in _read(filepath_or_buffer, kwds)
    480 
    481     # Create the parser.
--> 482     parser = TextFileReader(filepath_or_buffer, **kwds)
    483 
    484     if chunksize or iterator:

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in __init__(self, f, engine, **kwds)
    809             self.options["has_index_names"] = kwds["has_index_names"]
    810 
--> 811         self._engine = self._make_engine(self.engine)
    812 
    813     def close(self):

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\readers.py in _make_engine(self, engine)
   1038             )
   1039         # error: Too many arguments for "ParserBase"
-> 1040         return mapping[engine](self.f, **self.options)  # type: ignore[call-arg]
   1041 
   1042     def _failover_to_python(self):

~\.conda\envs\python_introduction\lib\site-packages\pandas\io\parsers\c_parser_wrapper.py in __init__(self, src, **kwds)
     67         kwds["dtype"] = ensure_dtype_objs(kwds.get("dtype", None))
     68         try:
---> 69             self._reader = parsers.TextReader(self.handles.handle, **kwds)
     70         except Exception:
     71             self.handles.close()

~\.conda\envs\python_introduction\lib\site-packages\pandas\_libs\parsers.pyx in pandas._libs.parsers.TextReader.__cinit__()

~\.conda\envs\python_introduction\lib\site-packages\pandas\_libs\parsers.pyx in pandas._libs.parsers.TextReader._get_header()

~\.conda\envs\python_introduction\lib\site-packages\pandas\_libs\parsers.pyx in pandas._libs.parsers.TextReader._tokenize_rows()

~\.conda\envs\python_introduction\lib\site-packages\pandas\_libs\parsers.pyx in pandas._libs.parsers.raise_parser_error()

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xd2 in position 7431: invalid continuation byte
```

Hier sieht man gut die Verschachtelung von vielen, vielen Funktionen/Methoden!


### Wo muss ich hinschauen?

Bei den ganzen Beispiel oben haben wir dazu schon viel gesehen. In den Fehlermeldungen stehen verschiedene Sachen drin, aber der wichtigste Hinweis kommt **ganz zum Schluss**. Dort steht was für eine Art Fehler vorliegt und häufig auch noch der Name der Funktion/Variable/Klasse... mit dem etwas nicht stimmt.

Häufige weitere Hinweise sind:

- `...'xyz' is not defined` heißt: Python kennt kein `xyz`.
  Meistens bedeutet das, dass entweder `xyz` bis zu dieser Stelle des Codes noch nicht definiert wurde (z.B. weil die entsprechenden Funktionen noch nicht aufgerufen wurden). Oder es liegt ein Tippfehler vor und es wurde aus versehen nicht etwas anderes definiert, vielleicht `Xyz` ? Oder `xyZ`? 

- `... object is not callable` bedeutet in der Regel, wir versuchen etwas auszuführen, was aber keine Funktion oder Methode. Darunter verstehen wir in Python das aufrufen eines Objekts mit runden Klammern, also z.B. `str(5)` oder `print("ja")`, oder auch `np.array(...)`. Gerade bei Komplexeren Klassen ist aber nicht immer sofort klar ob etwas eine Methode oder ein Attribut ist. Zum Beispiel in Pandas, dort haben wir nämlich zum Beispiel bei den DataFrames eine enorme Auswahl an Methoden **UND** Attributen. 

  ```python
  import pandas as pd
  
  df = pd.DataFrame({"a": [1, 2, 3],
                     "b": [4, 4, 1]})
  
  # DataFrames haben eine Unmenge an Methoden und Attributen, z.B.
  print(df.max())  # das ist eine Methode, darum mit ()
  print(df.values)  # das ist ein Attribut (genauer: eine "property")
  ```

  Das muss nicht immer logisch sein. Schließlich könnte `.values` auch genauso als Methode aufgefasst werden. Das ist in solch einem Fall einfach nur die Wahl der Entwickler*Innen. 
  (Randbemerkung: In andern Bibliotheken gibt es stattdessen eine Methode wie z.B: `.to_array()` bei `SciPy`)

  Da es aber beim DataFrame keine Methode ist, geht folgendes auch nicht:

  ```python
  df.values()
  
  TypeError                                 Traceback (most recent call last)
  ~\AppData\Local\Temp/ipykernel_15368/170898410.py in <module>
  ----> 1 df.values()
  
  TypeError: 'numpy.ndarray' object is not callable
  ```

  



