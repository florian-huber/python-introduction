### Variablen

Variablen sind essentiell für das Schreiben von Programmen.
Oft stellt man sich darunter Boxen vor in die Daten gepackt werden.

In Python werden Daten den Variablen einfach mit `=` zugewiesen.
```python 
a = 5
print(a / 6)  # => 0.8333333333333334
```
So lange eine Python Session läuft und nichts Anderes mit einer Variable
angestellt wird, behält die Variable den zugewiesenen Wert.
Zum Beispiel bleibt `a` der Wert 5 zugewiesen, so dass:
```python 
b = 8
print(a + b)  # => 13
```
Variablen sind aber nicht fix. 
Ihnen können einfach neue Werte zugewiesen werden
(sogar völlig andere Datentypen!):

```python 
a = 100.5
print(a + b)  # => 108.5
```
Meistens ist es sehr zu empfehlen, den Variablen Namen zu geben die einen
Hinweis auf ihren Inhalt enthalten.  
Code wie der folgende wird dadurch deutlich verständlicher!
```python 
training_seconds = 18900
sec_per_hour = 60 * 60
training_hours = training_seconds / sec_per_hour
print(training_hours)  # => 5.25
```
Python ist als Sprache sehr flexibel beim Umgang mit Variablen.
Oft ist das nützlich, kann aber gelegentlich auch für einiger Verwirrung
sorgen. Stellt euch vor eine Variable wird `print` genannt...
```python 
print = "my text"
```
Python erlaubt dies und gibt keinen Fehler aus.  
Keinen Fehler zu erhalten **heisst aber nicht, dass es eine gute Idee ist!**
Was wird wohl jetzt passieren wenn wir `print("hello world!")` laufen lassen?  

Ok, das sollte also deutlich sein... Variablen besser keinen Namen geben
der auch anderweitig als Funktion vorkommt. 

Eine Sache die Ptyhon doch verbietet sind Variablennamen die sogennanten 
[**reserverd keywords**](https://www.w3schools.com/python/python_ref_keywords.asp)
entsprechen
<!--pytest-codeblocks:expect-error-->
```python 
or = "outer region"  # => SyntaxError: invalid syntax
```
> ### Mini quiz:
> Was gibt der folgende Code aus?
```python 
x = 40
x = x + 2
print(x)
```
> a) 40  
> b) 42  
> c) x + 2  
> d) Error, da x = x + 2 mathematisch inkorrekt ist  
>

> Welche der folgenden Variablennamen sind OK?  
> a) `print = "my text"`  
> b) `else = "my text"`  
> c) `sehr_wichtig = "my text"`  
> d) `übermäßig_wichtig = "my text"`  
> e) `ImportantText = "my text"`  


### Regeln für Variablennamen
Namen mit Bedeutung
- Kleinbuchstaben (und Unterstriche)
- Nie "reserved keywords" (Schlüsselworte) benutzen
- Auch keine Funktionennamen benutzten (Standardfunktionen oder Funktionen die im Programm vorkommen)
- Andere Sprachen als Englisch sind natürlich OK (je nach Kontext)
- Allerdings machen Sonderzeichen manches komplizierter (z.B. ä oder ü)

