# VergleichProgrammiersprachen


## Compiler vs Interpreter
Beide agieren als Übersetzer des geschriebenen Codes in Maschinensprache. Die Art und Weise unterscheidet sich jedoch einander. Der Compiler nimmt den kompletten Code und übersetzt diesen. Der Interpreter hingegen übersetzt den Code Anweisung für Anweisung. Durchs sofortige übersetzen dauert es zwar etwas länger bis Kompiliervorgang abgeschlossen ist, ist dafür im Anschluss wesentlich schneller. Fehlersuche ist hingegen beim anweisungsweise übersetzen einfacher.

## C++

### Wie funktionieren Compiler und Linker
![C++ Funktionsweise](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/C%2B%2B%20Funktionsweise.png  "C++ Funktionsweise")

1. Präprozessorphase  
  Alle Präprozessoranweisungen fangen mit einem '#' an. Die bekanntesten sind: #include, #define und #if bzw #ifdef. Diese werden ausgeführt. Dies schließt auch das Einfügen der include-Dateien mit ein. Zudem werden kommentare gelöscht und Zeilen nur noch durch ein Newline-Zeichen getrennt
2. Frontendphase des Compilers  
  Sourcecode wird eingelesen und auf korrekter Syntax überprüft. Ziel ist eine erste Syntaxumwandlung und Optimierungen durchzuführen. Es wird ein Zwischencode erzeugt, der noch von dem Zielsystem unabhängig ist, aber dennoch die die Umsetzung in Assembler- oder Maschinensprache vorbereitet, indem die .cpp Dateien zu 'Translation Units' zusammengefügt werden.
3. Backendphase des Compilers  
  Einlesen des Zwischencodes sowie die Umsetzung der Assemblersprache mit dem Ziel einen Objektcode, der neben den Maschinencode auch Informationen zu den Daten und Programmabschnitten mitführt, zu generieren. Die 'Translation Units' werden zu einer .obj Datei.
4. Linkerphase  
  Der Linker ließt den Objektcode und Bibliotheken und fügt sie zusammen. Jetzt sind alle Adressen bekannt und der Maschinencode kann vervollständigt werden. Hierbei überprüft der Linker die einzelnen Verbindungen der Dateien, d. h. es wird geguckt ob alle Funktionen, die in einer Datei genutzt werden, in anderen Dateien gefunden werden können.  Ist dies nicht der Fall entsteht ein 'Linking Error': unresolved external symbols. Andersfalls ist der Output ein ausführbarer Maschinencode (.exe).

### Statisches und dynamisches linken
Beim statischen linken wird die Bibliothek (.lib) in das Programm gebunden (einamlig). Statisches linken ist technisch gesehen schneller.
Beim dynamischen linken wird die Bibliothek (.dll) zur Laufzeit verlinkt (immer wieder), d. h. wenn das Programm gestartet wird, wird geguckt ob die Bibliothek vorhanden ist.
Statisches Linken Beispiel: Zuerst ist ein neuer Ordner dem Projekt hinzuzufügen (hier: Include). In diesen Werden die Dateien hinein kopiert, welche gelinkt werden sollen. Im Beispiel wird lediglich die Datei x.h hinzugefügt. Um auf die Dateien zugreifen zu können, muss der Pfad des neuen Ordners in die Projekteigenschaften  unter C/C++ -> Allgemein -> Zusätzliche Includeverzeichnisse angegeben werden.

![Include](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten.jpg "Ordner hinzufügen")

Nun können die Funktionen spezifiziert und genutzt werden. Es fehlt lediglich noch:
```C++
#include <x.h>
```
Wenn nicht nur x.h sondern auf andere Dateien z. B. .lib Dateien inkludiert werden sollen, müssen diese dem Linker hinzugefügt werden: 
1. Pfad des Ordners der .lib Dateien zu Projekteigenschaften->Linker->Allgemein->Zusätzliche Bibliotheksverzeichnisse hinzufügen
![Allgemein](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten2.0.jpg "Ordner hinzufügen")
2. Namen der Dateien bei Projekteigenschaften->Linker->Eingabe->Zusätzliche Abhängigkeiten hinzufügen
![Eingabe](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten2.jpg ".lib Dateien hinzufügen")




