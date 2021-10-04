# Erste Schritte mit Listen und Sequenzen

## Strings
Strings sind Sequenzen aus Zeichen (oder Zeichenketten). Jedes einzelne
Zeichen eines strings kann in Python über einen Index angesprochen werden.

```python 
s = "Dies ist ein string"

print(s[0]) # => D
print(s[-1]) # => g
print(len(s)) # => 19
```
## Lists
Eine Liste (`list`) ist in Python eine Sammlung von Elementen.
Eine Liste wird einfach mit eckigen Klammern erstellt.
```python 
fruit = ["apple", "orange", "banana", "pear"]
```
Listen sind ebenfalls Sequenzen und einzelne Elemente können in Python
über Indices adressiert werden.

> ### Mini Quiz: Einmal Raten bitte
> Was gibt `print(fruit[1])` aus?  
> 1) SyntaxError  
> 2) apple  
> 3) orange  
>  
Was gibt `print(len(fruit))` aus?  
> 1) 1  
> 2) 4  
> 3) 21  

```python 
fruit = ["apple", "orange", "banana", "pear"]
print(fruit[2])  # => banana
```
Listen können völlig verschiedene Datentypen enthalten:
```python 
my_list = ["a string", 52, -0.014, "another string"]
print(my_list[0])  # => a string
```
Listen können sogar auch weiter Listen enthalten:
```python 
my_list = [3, [7, 2], "example"]
print(my_list[1])  # => [7, 2]
```
### Tuples
Ein Tuple ist in Python eine andere Form der Sammlung. Anders als `list`
wird ein Tuple mit runden Klammern erstellt. Die Elemente eines Tuples sind -genau wie Listen-
über Indices adressierbar:

```python 
fruit_tuple = ("apple", "orange", "banana", "pear")
print(fruit_tuple[2])  # => "banana"
```
Sieht eigentlich alles aus wie bei der Liste.  
Was ist jetzt da der Unterschied zur Liste?  
In Python unterscheiden wir veränderbare (**mutable**) und unveränderbare
(**immutable**) Datentypen. Listen zählen zu den veränderbaren, Tuple dagegen
sind unveränderbar.

```python 
fruit = ["apple", "orange", "banana", "pear"]
fruit[-1] = "mango"
print(fruit)  # => ['apple', 'orange', 'banana', 'mango']
```
<!--pytest-codeblocks:expect-error-->
```python 
fruit_tuple = ("apple", "orange", "banana", "pear")
fruit_tuple[-1] = "mango"  # => TypeError: 'tuple' object does not support item assignment
```
In einer Liste können wir also z.B. ein Element einfach ersetzen, bei 
einem Tuple geht das ganze aber nicht.

### Sequenzen
Tuple, list, und string sind alles Sequenzen. Damit gibt es eine ganze
Reihe von Funktionen (Methoden) die für alle drei Datentypen gleichermassen
eingesetzt werden können:
|     Operation        |     Ergebnis                                                                                                          |
|----------------------|-----------------------------------------------------------------------------------------------------------------------|
|     x in s           |     True wenn ein Element mit dem   Wert x in der Sequenz s enthalten ist, sonst False.                               |
|     x not in   s     |     False   wenn ein Element mit dem Wert x in der Sequenz s enthalten ist, sonst True.                               |
|     s + t            |     Konkatenation der beiden   Sequenzen s und t.                                                                     |
|     s * n            |     n Kopien der Sequenz s werden hintereinandergehängt.                                                              |
|     s[i]             |     Wiedergeben des i-ten Elementes   von s.                                                                          |
|     s[i:j]           |     Wiedergabe des Ausschnitts   (slice) von s, der vom i-ten bis zum j-ten Element (nicht einschließlich)   geht.    |
|     s[i:]            |     Wiedergabe des Ausschnitts   (slice) von s, der vom i-ten bis zum letzten Element geht.                           |
|     len(s)           |     Gibt die Länge der Sequenz s   wieder.                                                                            |
|     min(s)           |     Gibt das kleinste Element der   Sequenz s wieder.                                                                 |
|     max(s)           |     Gibt das größte Element der   Sequenz s wieder                                                                    |

Ein Beispiel:
```python 
my_list = [1, 4, 6, 20, 11, 99, 13, 1050]
my_str = "Ein String ist auch eine Sequenz."

4 in my_list # => True
"z" in my_str # => True

my_list[1:3] # => [4, 6]
my_str[1:3] # => in
```
Darüber hinaus haben die verschiedenen Datentypen aber noch weitere eigene Methoden
Das Arbeiten mit Strings werden wir in einigen der folgenden Vorlesungen noch
genauer betrachten. Hier aber schon einmal eine Tabelle mit wichtigen Methoden
für Python Listen.

### List Methoden

|     Operation                  |     Ergebnis                                                                                                          |
|--------------------------------|-----------------------------------------------------------------------------------------------------------------------|
|     my_list[i]   = x           |     Das Element mit Index i wird   durch x ersetzt.                                                                   |
|     my_list.append(x)          |     An die Liste s wird ein neues   Element x angehängt.                                                              |
|     my_list.extend(t)          |     Die Liste my_list   wird um die Element der Sequenz t verlängert.                                                 |
|     my_list.count(x)           |     Gibt die Anzahl der   Listenelemente mit dem Wert x zurück.                                                       |
|     del my_list   [i]          |     Das Element mit Index i wird aus   der Liste s entfernt, damit veringert   sich auch die Länge der Liste um 1.    |
|     my_list.index(x)           |     Gibt den kleinsten Index von my_list   zurück an dem ein Element gleich x ist.                                    |
|     my_list.insert(i,   x)     |     Falls i >= 0 wird x vor dem   Index i in die Liste my_list eingefügt.                                             |
|     my_list.remove(x)          |     Das erste Element gleich x wird   aus der Liste my_list entfernt.                                                 |
|     my_list.sort()             |     Die Elemente der Liste werden   aufsteigend sortiert                                                              |
|     max(s)                     |     Gibt das größte Element der   Sequenz s wieder                                                                    |


