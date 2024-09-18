# Clean code und "best practices"

In den ersten Kapitels haben wir schon viele wichtige Grundlagen in Python gesehen:

* Verschiedene **Datentypen**: Zahlen, Strings, List, Tuple, Dictionary, Set, Boolean, None
* `for`- und `while`-Schleifen
* Verzweigungen: if-elif-else
* Bedingungen
* Funktionen
* Fehlerhandling mit try-except
* Bibliotheken

Höchste Zeit um einen kleinen Schritt zurück zu machen und den Blick freizugeben für das **big picture**, das große Ganze. 

## Teams vs. lonely hacker

Wir haben etwas ausführlicher über Teams und Pair Programming gesprochen --> bisher nur in den Slides auf Moodle zu finden.

Danach ging es um **Code Quality** und **Clean Code**, angefangen mit:

### Zen of Python

Bitte einmal den folgenden `import` ausprobieren:

```python
import this
```

Was Folgendes liefern sollte:

```
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

Darüber wie heilig oder ironisch diese  Zen-Regeln sind, und darüber was genau sie bedeuten sollen, gibt es unterschiedliche Meinungen.

Ein Großteil der Regeln bezieht sich darauf, den Code so zu schreiben, dass er gut lesbar und verständlich ist. Was aber genau nun aber an Code schön (*beautiful*) oder hässlich (*ugly*) ist, was genau gut oder nicht so gut lesbar ist, darüber gibt es in der Praxis ziemlich verschiedenen Meinungen. Vor allem für Programmier-Anfänger*Innen sind solche Style-Fragen oft schwer zu beantworten, zum einen wegen der noch fehlenden Erfahrung (also weniger Vergleichsmöglichkeiten), aber auch weil es ja schon schwierig genug ist Code zum laufen zu bringen! Warum muss der dann auch noch *schön* sein?!

Darum müsste das **Zen of Python** für Programmier-Anfänger*Innen eigentlich besser beginnen mit:

```
Working code is a big achievement! (no matter how ugly)
Beautiful is better than ugly.
...
```

Auch wenn die Schönheit des Codes v.a. zu Beginn kein guter Maßstab ist. Lesbaren, verständlichen Code zu schreiben, ist auch für Programmier-Anfänger*Innen ein gutes Ziel.

## Clean Code / Best practices

Wir hatten zuvor schon einige Dinge zu lesbarem Code besprochen:

+ Sinnvolle Variablen- und Funktionennamen
+ Nutzung von Funktionen (auch lokalen Funktionen) um Code besser zu strukturieren.
+ Code Dokumentation durch Kommentare im Code oder Docstrings bei Funktionen.

Nochmal ein Beispiel dazu: Was macht der folgende Code:

```python
def handle_x(x):
    b = 9 / 5
    c = 32
    d = 273.15
    e = x - d
    return e * b + c

print(handle_x(400))
```

Na? ...



Hier nochmal das gleiche Beispiel mit besseren Variablen- und Funktionsnamen:

```python
def kelvin_to_fahrenheit(temp_kelvin: float):
    """Umrechnen von Temperatur in Kelvin zu Fahrenheit.
    """
    fahrenheit_factor = 9 / 5
    fahrenheit_offset = 32
    absolute_zero = 273.15
    temp_celsius = temp_kelvin - absolute_zero
    return temp_celsius * fahrenheit_factor + fahrenheit_offset

print(kelvin_to_fahrenheit(400))
```

Auch jetzt ist vielleicht nicht unbedingt auf den ersten Blick jeder Schritt der Funktion sofort klar. Aber es wird sofort deutlich was die Funktion überhaupt machen soll ("Umrechnen von Temperaturen in Kelvin zu Fahrenheit"). Und auch die Rechenschritte können nun theoretisch überprüft werden, da klar ist was die Variablen zu bedeuten haben.

## Beispiel: Rock-Paper-Scissors

Wir wollen jetzt das Spiel Rock-Paper-Scissors (Stein-Schere-Papier) programmieren. Zuerst soll ein menschlicher Spieler gegen den Computer antreten. 

Dafür wir fangen einmal damit an, eine Funktion zu schreiben in der der Computer zufällig eine der drei Möglichkeiten auswählt. Für das Zufallselement nutzen wir wieder die Bibliothek `random`.

```python
import random

def rock_paper_scissor():
    """Returns randomly chosen string: "rock", "paper", or "scissor".
    """
    choice = None
    random_number = random.randint(1, 3)
    if random_number == 1:
        choice = "rock"
    elif random_number == 2:
        choice = "paper"
    else:
        choice = "scissor"
    return choice
```

Die Funktion und die Variablen haben sinnvolle Namen, ein Docstring beschreibt ungefähr was die Funktion macht. Also alles in Ordnung, oder?

Im Prinzip schon. Der Code läuft und man kann ihn verstehen. Aber es geht noch etwas besser:

```python
def rock_paper_scissor():
    """Returns randomly chosen string: "rock", "paper", or "scissor".
    """
    random_number = random.randint(1, 3)
    if random_number == 1:
        return "rock"
    if random_number == 2:
        return "paper"
    return "scissor"
```

Warum besser?
Das wäre so etwas wie das *Simple is better than complex.* aus dem Python-Zen (und vielleicht ist der Code auch hübscher?). Wir geben hier nämlich direkt die entsprechenden Strings über `return` zurück. Damit brauchen wir kein elif-else mehr. Und auch die Variable `choice` fällt dadurch weg.

Wir können die Funktion aber auch anders aufbauen, z.B. mit `random.choice()`. 

```python
import random

def rock_paper_scissor():
    """Returns randomly chosen string: "rock", "paper", or "scissor".
    """
    choice = random.choice(["a", "b", "c"])
    if choice == "a":
        return "rock"
    if choice == "b":
        return "paper"
    return "scissor"
```

Wie wäre das? 

Besser? Schlechter? Anders?

Wenn man aber schon weiß, dass mit `random.choice()` ein String ausgewählt werden kann, dann geht es auch noch ein ganzen Stück einfacher/eleganter:

```python
import random

def rock_paper_scissor():
    """Returns randomly chosen string: "rock", "paper", or "scissor".
    """
    return random.choice(["rock", "paper", "scissor"])
```

OK, in dem Fall sollte deutlich sein, dass der Code einfacher und eleganter geworden ist.
Die Lösung davor, die erst a, b, oder c auswählt macht nämlich einen völlig unnötigen Umweg.

Um aber auf solch eine Lösung zu kommen, muss natürlich überhaupt erstmal bekannt (und präsent) sein, dass es `random.choice()` gibt und dass es so eingesetzt werden kann! In Python ist genau so eine Situation recht typisch. Irgendwo, in irgendeiner Bibliothek gibt es eine Funktion die das macht was gerade benötigt wird. Schön, wenn das gerade hinhaut. Aber keine Sorge: Niemand kennt alle diese Möglichkeiten und darum schreiben unzählige Python-Programmieren*Innen täglich kleine Funktionen selbst, auch wenn es vielleicht irgendwo dafür schon eine Bibliothek gibt. Auch die erste Lösung-Strategie über eine Zufallszahl mit `random.randint()` ist daher völlig OK. 

Aber jetzt: Weiter im Spiel!

Wie baue ich jetzt ein ganzen Rock-Paper-Scissors Spiel?

Natürlich kann man einfach los-programmieren. Für solch ein einfaches Spiel geht das auch noch halbwegs:

<!-- pytest-codeblocks:skip -->

```python
def game():
    print("Yes! Rock-Paper-Scissors!")
    computer_choice = rock_paper_scissor()
    print("What is your choice?")
    human_choice = input("a) rock, b) paper, c) scissor ... :")
    if human_choice.lower() in ["a", "b", "c"]:
        if human_choice == "a":
            if computer_choice == "paper":
                print("Computer wins!")
            elif computer_choice == "scissor":
                print("Human wins!")
            else:
                print("Drawn...")
        elif human_choice == "b":
            if computer_choice == "scissor":
                print("Computer wins!")
            elif computer_choice == "rock":
                print("Human wins!")
            else:
                print("Drawn...")
        else:
            if computer_choice == "rock":
                print("Computer wins!")
            elif computer_choice == "paper":
                print("Human wins!")
            else:
                print("Drawn...")   
    else:
        print("Invalid input!")
            
            
game()
```

Das funktioniert auch. Aber es könnte ruhig etwas lesbarer und besser strukturiert sein.

Ein Zen-Spruch der hier zutreffen würde ist *Flat is better than nested*. Denn wir sehen hier drei verschachtelte`if`-Verzweigungen!

Die äußere `if`Verzweigung soll nur sicherstellen, dass keine falsche Eingabe stattgefunden hat. Das lässt sich aber auch mit `assert`erledigen:

<!-- pytest-codeblocks:skip -->

```python
def game():
    print("Yes! Rock-Paper-Scissors!")
    computer_choice = rock_paper_scissor()
    print("I made my choice. What is yours?")
    human_choice = input("a) rock, b) paper, c) scissor ... :")
    assert human_choice.lower() in ["a", "b", "c"], "Invalid user input."
    
    if human_choice == "a":
        if computer_choice == "paper":
            print("Computer wins!")
        elif computer_choice == "scissor":
            print("Human wins!")
        else:
            print("Drawn...")
    elif human_choice == "b":
        if computer_choice == "scissor":
            print("Computer wins!")
        elif computer_choice == "rock":
            print("Human wins!")
        else:
            print("Drawn...")
    else:
        if computer_choice == "rock":
            print("Computer wins!")
        elif computer_choice == "paper":
            print("Human wins!")
        else:
            print("Drawn...")   
            
            
game()
```

Ok. Schon besser.

Besser lesbar - und wie wir nachher sehen werden auch rubuster und vielseitiger- sind oft Lösungen bei denen der Code in kleinere Teile, z.B. Funktionen gegliedert ist.

Welche Funktionen könnten wir hier getrennt definieren?

Wie wäre es z.B. mit einem folgenden Schema:

`computer_choice`--> `player_choice`-->`who_won`-->`print result` ?

Den ersten Teil haben wir schon mit unserer Funktion `rock_paper_scissor()` abgedeckt. 

Ein Vorschlag für den Rest wäre der folgende Code:

<!-- pytest-codeblocks:skip -->

```python
def user_choice():
    print("What is your choice?")
    human_choice = input("a) rock, b) paper, c) scissor ... :")
    choices_dict = {"a": "rock",
                    "b": "paper",
                    "c": "scissor"}
    assert human_choice in ["a", "b", "c"], "Invalid user input."    
    return choices_dict[human_choice]
    
def who_won(hand1, hand2):
    """Check which player wins.
    """
    if hand1 == hand2:  # Drawn
        return 0
    # Other combinations
    endings = {"rock-paper": -1,
                "rock-scissor": 1,
                "paper-rock": 1,
                "paper-scissor": -1,
                "scissor-paper": 1,
                "scissor-rock": -1}
    return endings[f"{hand1}-{hand2}"]


def play_game():
    """Play one round of rock-paper-scissors against the computer.
    """      
    print("Yes! Rock-Paper-Scissors!")
    
    computer_choice = rock_paper_scissor()
    human_choice = user_choice()
    print(f"Human: {human_choice} vs. computer: {computer_choice}.")
    
    winner = who_won(human_choice, computer_choice)
    if winner == 0:
        print("Drawn... no winner")
    elif winner == 1:
        print("Human won!")
    else:
        print("Computer wins!")  
            
            
play_game()
```

Der Code sieht erstmal nicht unbedingt einfacher aus!

Allerdings hat er trotzdem einige Vorteile:

+ der Code von `play_game()` reicht aus um den Code-Ablauf zu verstehen (die Details sind nun alle in den entsprechenden Funktionen "versteckt")
+ Die Funktionen sind so definiert, dass schnell Änderungen am Spiel vorgenommen werden können!
  Zum Beispiel gibt `who_won()` zurück wer gewonnen hat, unabhängig davon ob es um einen Computer oder einen menschlicher Spieler geht! Damit könnte schnell eine Variante programmiert werden, bei der zwei menschliche Spieler gegeneinander spielen --> `play_game_two_players()`o.ä.
+ Die Bedingungen um zu Gewinnen sind hier deutlich lesbarer.
  Damit können sie auch viel einfacher verändert oder ergänzt werden.

Apropos ergänzen: Mit den Code-Aufbau über die Funktionen ist es nun auch gut möglich das Spiel Rock-Paper-Scissors zu verändern zum Spiel **Rock-Paper-Scissors-Lizard-Spock** (siehe Serie [Big Bang Theory](https://commons.wikimedia.org/wiki/File:Rock_paper_scissors_lizard_spock.svg))



## Mehr zum Python-Ecosystem

In der ersten Vorlesung wurde schon einmal erwähnt, dass um Python herum ein ganzes Universum aus Bibliotheken und Tools existiert, was ganz nebenbei auch einer der Gründe für den Erfolg von Python ist.

Bisher haben wir ausschließlich mit Standard-Bibliotheken gearbeitet, die bei einer normalen Python Installation gleich mit dabei sind.

D.h. wenn wir `import time` oder `import random` aufgerufen haben, wurde die entsprechende Bibliothek einfach importiert und stand zur Verfügung. Das ist aber nicht die Regel. Sehr viele Bibliotheken müssen erst installiert werden.

Ist eine Bibliothek noch nicht installiert liefert dies ansonsten einen `ModulNotFoundError`.

```
>>> import seaborn as sns

ModuleNotFoundError: No module named 'seaborn'
```

Also: Wo kommen diese Bibliotheken her? Wie installiert man sie? Und wie hängen sie voneinander ab?

Vereinfacht ist es in etwa so:
Der Quellcode (source code) der einzelnen Bibliotheken lebt irgendwo im Internet, in den meisten Fällen auf GitHub oder verwandten Plattformen. Die jeweiligen "releases", d.h. die verschiedenen veröffentlichten Versionen davon werden zudem von sogenannten Package-Managing Systemen verwaltet, v.a. **conda** und **pypi**. Je nachdem über welches System eine Bibliothek installiert werden soll, bzw. je nachdem auf welcher der Plattformen eine Bibliothek verfügbar ist, können Bibliotheken  von der Konsole installiert werden :

```
pip install seaborn
```

oder

```
conda install seaborn
```

### Bereits installierte Bibliotheken

Um zu sehen, welche Bibliotheken bereits installiert sind, kann im Terminal `pip list` oder `conda list` ausgeführt werden. Dabei wird deutlich, dass (1) es ziemlich viele Bibliotheken gibt und (2) das alle Bibliotheken auch in bestimmten Versionen vorliegen!

Und damit wären wir auch an einem der Probleme mit den Bibliotheken angekommen.

Viele Python Bibliotheken verwenden wiederum selbst andere Python Bibliotheken, das sind dann "dependencies", Abhängigkeiten der Module. Das Problem dabei ist nur, dass unterschiedliche Bibliotheken manchmal unterschiedliche Versionen benötigen. Es kann daher sein das eine Bibliothek `module_X` benötigt mit einer Version `<3.0.0` während eine andere Bibliothek `module_X` mit einer Version `>3.0.2` braucht um zu laufen. In solch einem Fall können nicht beide Bibliotheken gleichzeitig installiert werden!

Um solchen Konflikten so weit es geht aus dem Weg zu gehen, verwenden viele Nutzer*Innen sogenannte **Environments**, also Umgebungen, die es erlauben verschiedene Python-Ecosystems parallel installiert zu haben. Wir werden zu einem späteren Zeitpunkt dafür **conda** nutzen.

