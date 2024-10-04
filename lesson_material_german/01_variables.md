# Variablen

Variablen sind ein essenzieller Bestandteil jedes Programms. Sie ermöglichen es uns, Daten zu speichern und in Berechnungen zu verwenden. Eine häufige Metapher ist, sich Variablen wie Boxen vorzustellen, in die wir Werte ablegen können. In Python und anderen Programmiersprachen wird dabei jedoch präziser von **Zuweisungen** gesprochen: Einem Variablennamen wird ein Wert zugewiesen.

## Variablen in Python

In Python wird eine Variable durch das Zuweisungszeichen `=` definiert. Der Ausdruck links vom `=` ist der Variablenname, der Ausdruck rechts ist der Wert, der dieser Variablen zugewiesen wird.

Beispiel:

```python 
a = 5
print(a / 6)  # => 0.8333333333333334
```

Sobald eine Variable definiert ist, "merkt" sich Python ihren Wert während der gesamten Programmsitzung. Die Variable `a` hat im obigen Beispiel den Wert `5`, bis ihr etwas anderes zugewiesen wird. Variablen können dann in Berechnungen oder anderen Operationen verwendet werden:

<!--pytest-codeblocks:cont-->

```python 
b = 8
print(a + b)  # => 13
```

### Variablen können ihren Wert ändern

Variablen sind in Python nicht fixiert. Ihr könnt ihnen jederzeit neue Werte zuweisen, sogar von einem anderen Datentyp. Das bedeutet, dass eine Variable, die zuvor eine ganze Zahl (`int`) war, plötzlich einen Gleitkommawert (`float`) oder sogar einen String speichern kann:
<!--pytest-codeblocks:cont-->

```python 
a = 100.5
print(a + b)  # => 108.5
```

Wie gerade erwähnt, kann der Datentyp nach belieben verändet werden, also ist auch das folgende problemlos möglich:
```python
a = 100
print(2 * a)

# Jetzt die Variable überschreiben (eigentlich: "neu zuweisen")
a = "100"
print(2 * a)
``` 

![Assigning new datatype to variable](../images/python_assign_variables_change_datatype.png)

```{note}
**Achtung**: Der flexible Umgang mit Variablen in Python ist nützlich, kann aber zu Fehlern führen, wenn man versehentlich den Typ einer Variablen ändert und später in einem Programm aber den ursprünglichen Datentyp erwartet.
```



### Sinnvolle Variablennamen

Es ist sehr hilfreich Variablen Namen zu geben, die den Inhalt oder den Zweck der gespeicherten Daten widerspiegeln. Ein selbsterklärender Variablenname macht den Code für andere (und auch für euch selbst) leichter verständlich.

```python 
training_seconds = 18900
sec_per_hour = 60 * 60
training_hours = training_seconds / sec_per_hour
print(training_hours)  # => 5.25
```

Hier sind die Variablen klar benannt, und der Code ist direkt verständlich. Würde man stattdessen Variablen wie `a`, `b` und `c` verwenden, wäre der Code weniger leserlich und schwieriger zu verstehen.

### Gefährliche Variablennamen

Python erlaubt es, Variablen fast jeden Namen zu geben. Dies gilt sogar für Namen, die den Namen von Python-Funktionen entsprechen, was jedoch vermieden werden sollte. Wenn ihr beispielsweise einer Variablen den Namen `print` gebt, überschreibt ihr die eingebaute Funktion `print()` und könnt sie nicht mehr verwenden:

```python 
print = "my text"
```

Python erlaubt dies und gibt keinen Fehler aus.  
Keinen Fehler zu erhalten **heisst aber nicht, dass es eine gute Idee ist!**
Was wird wohl jetzt passieren wenn wir `print("hello world!")` laufen lassen?  

Ok, das sollte also deutlich sein... Variablen besser keinen Namen geben
der auch anderweitig als Funktion vorkommt. 

### Reservierte Schlüsselwörter

Python hat eine Liste von **reservierten Schlüsselwörtern** (Keywords), die nicht als Variablennamen verwendet werden dürfen, da sie für die Syntax der Sprache benötigt werden. Beispiele für solche Keywords sind `if`, `else`, `while` oder `for`.

Der Versuch, eine Variable mit einem solchen Namen zu erstellen, führt zu einem **SyntaxError**:

<!--pytest-codeblocks:expect-error-->

```python 
or = "outer region"  # => SyntaxError: invalid syntax
```
Die vollständige Liste der reservierten Schlüsselwörter findet ihr [hier](https://www.w3schools.com/python/python_ref_keywords.asp).

---

> ## Mini quiz:
>
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

> Welche der folgenden Variablennamen sind OK?  
> a) `print = "my text"`  
> b) `else = "my text"`  
> c) `sehr_wichtig = "my text"`  
> d) `übermäßig_wichtig = "my text"`  
> e) `ImportantText = "my text"`  


## Regeln für Variablennamen

Es gibt einige einfache, aber wichtige Regeln für die Wahl von Variablennamen in Python:

- **Verwendet Kleinbuchstaben** und Unterstriche `_` zur Trennung von Wörtern, um die Lesbarkeit zu erhöhen.
- **Vermeidet reservierte Keywords** oder Funktionsnamen, um Konflikte zu verhindern.
- **Sprecht Englisch**: Auch wenn Variablennamen in anderen Sprachen (z. B. Deutsch) zulässig sind, wird Englisch üblicherweise bevorzugt, um den Code auch für andere verständlich zu halten.
- **Achtet auf Sonderzeichen**: Vermeidet nach Möglichkeit Umlaute (ä, ö, ü) oder andere Sonderzeichen in Variablennamen, da sie nicht überall unterstützt werden und zu Komplikationen führen können.



## Debugging!

Mit **debugging** meinen wir das Beseitigen von Fehlern im Programmcode (Fehler = "bugs").
Wir werden später noch detaillierter auf das Thema eingehen, aber gleich zu Beginn ist es hilfreich ein wenig Erfahrung zu sammeln mit der Art und Weise wie Python (bzw. der Python-Interpreter) Fehler im Code anzeigt. Hierbei geht es nicht um Fehler, die ein erfolgreiches Ausführen des Codes unmöglich machen.

### Übung zum Debugging:

1. Was passiert bei einer print-Anweisung, wenn wir eine oder beide 
Klammern weglassen?

2. Was passiert wenn wir einen String ausgeben wollen und eine 
oder beide Anführungszeichen weglassen?

3. Was passiert wenn wir zwei Zahlen addieren wollen aber ++ oder +++ schreiben?

4. Was passiert wenn wir ein + oder eine Null vor 
eine Zahl schreiben, z.B. 05 - 2 oder +5 -2 ?

5. Was passiert wenn wir ein Leerzeichen vor print("hello") schreiben?

6. Mit Python beantworten ("Taschenrechnermodus"): Wie viele Sekunden 
haben 42 Minuten und 42 Sekunden?
