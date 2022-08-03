# VergleichProgrammiersprachen


## Compiler vs Interpreter
Beide agieren als Übersetzer des geschriebenen Codes in Maschinensprache. Die Art und Weise unterscheidet sich jedoch voneinander. Der Compiler nimmt den kompletten Code und übersetzt diesen. Der Interpreter hingegen übersetzt den Code Anweisung für Anweisung. Durchs sofortige übersetzen dauert es zwar etwas länger bis Kompiliervorgang abgeschlossen ist, ist dafür im Anschluss wesentlich schneller. Fehlersuche ist hingegen beim anweisungsweise Übersetzen einfacher.

## C++

### Wie funktionieren Compiler und Linker
![C++ Funktionsweise](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/C%2B%2B%20Funktionsweise.png  "C++ Funktionsweise")

1. Präprozessorphase  
  Alle Präprozessoranweisungen fangen mit einem '#' an. Die bekanntesten sind: #include, #define und #if bzw #ifdef. Diese werden ausgeführt. Dies schließt auch das Einfügen der include-Dateien mit ein. Zudem werden kommentare gelöscht und Zeilen nur noch durch ein Newline-Zeichen getrennt
2. Frontendphase des Compilers  
  Sourcecode wird eingelesen und auf korrekter Syntax überprüft. Ziel ist eine erste Syntaxumwandlung und Optimierungen durchzuführen. Es wird ein Zwischencode erzeugt, der noch von dem Zielsystem unabhängig ist, aber dennoch die Umsetzung in Assembler- oder Maschinensprache vorbereitet, indem die .cpp Dateien zu 'Translation Units' zusammengefügt werden.
3. Backendphase des Compilers  
  Einlesen des Zwischencodes sowie die Umsetzung der Assemblersprache mit dem Ziel einen Objektcode, der neben den Maschinencode auch Informationen zu den Daten und Programmabschnitten mitführt, zu generieren. Jede 'Translation Unit' wird zu einer .obj Datei.
4. Linkerphase  
  Der Linker ließt den Objektcode und die Bibliotheken und fügt sie zusammen. Jetzt sind alle Adressen bekannt und der Maschinencode kann vervollständigt werden. Hierbei überprüft der Linker die einzelnen Verbindungen der Dateien, d. h. es wird geguckt ob alle Funktionen, die in einer Datei genutzt werden, in anderen Dateien gefunden werden können.  Ist dies nicht der Fall entsteht ein 'Linking Error': unresolved external symbols. Andersfalls ist der Output ein ausführbarer Maschinencode (.exe).

### Statisches und dynamisches linken
Beim statischen Linken wird die Bibliothek (.lib) in das Programm gebunden (einamlig). Statisches Linken ist technisch gesehen schneller.
Beim dynamischen Linken wird die Bibliothek (.dll) zur Laufzeit verlinkt (immer wieder), d. h. wenn das Programm die Bibliothek benötigt, wird geguckt ob die Bibliothek vorhanden ist.
Statisches Linken Beispiel: Zuerst ist ein neuer Ordner dem Projekt hinzuzufügen (hier: Include). In diesen Werden die Dateien hinein kopiert, welche gelinkt werden sollen. Im Beispiel wird lediglich die Datei x.h hinzugefügt. Um auf die Dateien zugreifen zu können, muss der Pfad des neuen Ordners in die Projekteigenschaften  unter C/C++ -> Allgemein -> Zusätzliche Includeverzeichnisse angegeben werden.

![Include](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten.jpg "Ordner hinzufügen")

Nun können die Funktionen spezifiziert und genutzt werden. Es fehlt lediglich noch:
```C++
#include <x.h>
```
Wenn nicht nur x.h sondern auch andere Dateien z. B. .lib Dateien inkludiert werden sollen, müssen diese dem Linker hinzugefügt werden: 
1. Pfad des Ordners der .lib Dateien zu Projekteigenschaften->Linker->Allgemein->Zusätzliche Bibliotheksverzeichnisse hinzufügen
![Allgemein](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten2.0.jpg "Ordner hinzufügen")
2. Namen der Dateien bei Projekteigenschaften->Linker->Eingabe->Zusätzliche Abhängigkeiten hinzufügen
![Eingabe](https://github.com/JoBo33/VergleichProgrammiersprachen/blob/main/InkedC%2B%2BCompilerLinker-Eigenschaftenseiten2.jpg ".lib Dateien hinzufügen")

Das dynamischen Linken läuft ziemlich gleich ab. Es muss lediglich die .dll.lib Datei (anstatt die .lib Datei) bei den zusätzlichen Abhängigkeiten hinzugefügt werden und die .dll Datei muss in den Ordner der .exe datei kopiert werden.

### Speicherverwaltung
Es gibt zwei Arten von Speicher - Stack und Heap. Beim Stack muss man den Speicher weder anfordern noch freigeben. Der Speicher wird von Variabeln und Objekten nur solange belegt, wie diese sichtbar sind. D. h. sobald aus dem Scope gegangen wird, wird der ganze Speicher der innerhalb des Scopes neu gebraucht wurde automatisch wieder frei. Ausnahmen sind Variablen mit dem Schlüsselwort static. Werden diese innerhalb eines Scopes deklariert, so wird die Variable nur einmal erzeugt und auf einer festen Adresse gespeichert. Wenn der Scope verlassen wird, ist sie zwar nicht mehr sichtbar, aber sie wird auch nicht direkt zerstört. Globale Variablen und statische Variablen werden erst am Ende des Programms zerstört.  
Beispiel:
```C++
{
    int a = 5;
}
```
Im Beispiel wird Variable a initialisiert, es ist ein Integer (4 Bytes). Das heißt es werden 4 Bytes für den Integer reserviert und der Stack Pointer um 4 Bytes verschoben. Die einzelnen Variablen werden beim Stack aufeinander gespeichert (wie bei einem Stapel). Daher ist speichern auf den Stack sehr schnell.  
  
Beim Heap handelt es sich um dynamischen Speicher. Der Speicher muss explizit angefordert (new) und freigegeben (delete) werden. Das bedeutet der angeforderte Speicher wird solange reserviert bis es explizit mit delete freigegeben wird.  Beim Heap werden die Variabeln irgendwo im Speicher gespeichert.

Beispiel:
```C++
{
    int *a = bew int [5];       //anfordern
    for (int i = 0; i < 5; i++)
    {
        a[i] = i;
    }
    delete[] a;               // freigeben
}
```

Wenn immer es geht, ist es sinnvoll Vaiabeln auf dem Stack zu speichern, da es nur eine CPU Anweisung ist (Bei Klassen 2 Anweisungen). Beim Heap hingegen wird der Operator new bzw delete aufgerufen, wodurch wesentlich mehr Anweisungen ausgeführt werden müssen.

Smart Pointer:  
Bei Smart Pointer werden die Schlüsselworter new und delete nicht benötigt, da die Pointer automatisch gelöscht werden, wenn diese nicht mehr benötigt werden. Wann dies der Fall ist hängt von der Smart Pointer Art ab. Um Smart Pointer nutzen zu können braucht man: 
```C++
#include <memory>
```
unique_ptr:  
Können nicht kopiert werden. Es wird auf dem stack angelegt und wird somit gelöscht wenn aus dem Scope gegangen wird.  
shared_ptr:  
Bei einem Shared Pointer wird die Anzahl der Referenzen gezählt. Diese wird in einem anderen Speicherblock (Kontrollblock) gespeichert. Sobald der Zähler auf 0 fällt wird der Speicher freigegeben und der Pointer gelöscht.  
weak_ptr:  
Weak Pointer können genutzt werden wie Shared Pointer. Der wesentliche Unterschied ist jedoch, dass sie das Objekt nicht am leben halten, da sie den Zähler nicht erhöhen. D. h. wird ein Weak Pointer mit einem Shared Pointer initialisiert wird das Objekt freigegeben sobald man nicht mehr im Scope des Shared Pointers ist.  





