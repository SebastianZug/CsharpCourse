<!--

author:   Sebastian Zug, Galina Rudolf, André Dietrich & `Lina`
email:    sebastian.zug@informatik.tu-freiberg.de
version:  1.0.0
language: de
narrator: Deutsch Female

import: https://github.com/liascript/CodeRunner

icon: https://upload.wikimedia.org/wikipedia/commons/d/de/Logo_TU_Bergakademie_Freiberg.svg
-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://github.com/TUBAF-IfI-LiaScript/VL_Softwareentwicklung/blob/master/05_CsharpGrundlagenIII.md)

# C\# Grundlagen III

| Parameter                | Kursinformationen                                                                                                                                                                          |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Veranstaltung:**       | `Vorlesung Softwareentwicklung`                                                                                                                                                            |
| **Semester**             | `Sommersemester 2021`                                                                                                                                                                      |
| **Hochschule:**          | `Technische Universität Freiberg`                                                                                                                                                          |
| **Inhalte:**             | `Einführung in die Basiselemente der Programmiersprache C# - Eingabe/Ausgabe, Ausnahmen`                                                                                                                                |
| **Link auf den GitHub:** | [https://github.com/TUBAF-IfI-LiaScript/VL_Softwareentwicklung/blob/master/05_CsharpGrundlagenIII.md](https://github.com/TUBAF-IfI-LiaScript/VL_Softwareentwicklung/blob/master/05_CsharpGrundlagenIII.md) |
| **Autoren**              | @author                                                                                                                                                                                    |

![](https://media.giphy.com/media/26tn33aiTi1jkl6H6/source.gif)

---------------------------------------------------------------------

## Ein und Ausgabe

## Schreiboperation

C# selbst besitzt keine Anweisungen für die Ein- und Ausgabe von Daten, dazu
existieren aber mehrere Bibliotheken, die im folgenden für die Bildschirmausgabe
und das Schreiben in Dateien vorgestellt werden sollen.

Für das Schreiben stehen zwei Methoden `System.Console.Write` und `System.Console.WriteLine`.

<!-- --{{0}}-- Idee des Codeblockes
  * Erinnerung an Steuerzeichen /n /t
 -->

```csharp   Write/WriteLine
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
      Console.Write("One");
      Console.Write("Two");
      Console.WriteLine("Three");
      Console.WriteLine();
      Console.WriteLine("Four");
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

Diese decken erstens eine **große Bandbreite von Übergabeparametern** und bedienen
zweitens verschiedene **Ausgabeschnittstellen**.

| WriteLine() Methoden                                    | Anwendung                                 |
| ------------------------------------------------------- | ----------------------------------------- |
| `WriteLine()`                                             | Zeilenumbruch                             |
| `WriteLine(UInt64), WriteLine(Double), WriteLine(Object)` | Ausgabe von Variablen der Basisdatentypen |
| `WriteLine(Object)`                                       |                                           |
| `WriteLine(String)`                                       | Ausgabe einer `string` Variable           |
| `WriteLine(String, Object)`                               | String und Formatinformationen            |
| `WriteLine(String, Object, Object)`                       |                                           |
| `WriteLine(String, Object, Object, Object)`               |                                           |
| `WriteLine(String, Object[])`                             |                                           |

### Kombinierte Formatierung von Strings

Die "Kombinierte Formatierung" unter C# ermöglicht eine breite Festlegung bezüglich des Formats der Ausgaben. Die folgenden Aussagen beziehen sich dabei aber nicht
nur auf die Anwendung im Zusammenhang mit `WriteLine()` und `Write()` sondern
können auch auf:

* [String.Format](https://docs.microsoft.com/de-de/dotnet/api/system.string.format) (und damit ToString()!)
* [String.Builder](https://docs.microsoft.com/de-de/dotnet/api/system.text.stringbuilder)
* [Debug.WriteLine](https://docs.microsoft.com/de-de/dotnet/api/system.diagnostics.debug.writeline#System_Diagnostics_Debug_WriteLine_System_String_System_Object___)
...

angewandt werden.

Dabei sind folgende Ausdrücke möglich:

```
"{n}"
"{n, width}"
"{n, width:format}"
"{n, width:format precision}"
     //ohne Lehrzeichen zwischen format und precision!
```

<!-- --{{0}}-- Idee des Codeblockes
  * Diskussion der Indizes, Durchtauschen der Indizes,
  * Erzeugung Fehler Indizes
  * Darstellung der Breite
  * Positives negatives Vorzeichen der Breite
  * Angabe der Formate, precision
 -->

Probieren sie es doch in folgendem Beispiel mal aus:

```csharp   IndizesKombinierteFormatierung
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
       int ivalue = 56;
       double dvalue = 43.2234;
       bool bvalue = true;
       Console.WriteLine("Test {0:e}, {1}", dvalue, bvalue);
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

| Format Symbol | Bedeutung                | Beispiel      |
| ------------- | ------------------------ | ------------- |
| G             | Default                  |               |
| E, e          | Expoentiell              | 1.052033E+003 |
| X, x          | Hexadezimal              | 1FF           |
| P, p          | Prozent                  | -38.8         |
| D, d          | Dezimal                  | 1231          |
| N, n          | Dezimal mit Trennzeichen | 1.23432,12    |

Eine komplette Auflistung findet sich unter https://docs.microsoft.com/de-de/dotnet/standard/base-types/standard-numeric-format-strings

> Achtung: Die Formatzeichen sind typspezifisch, es exisiteren analoge Zeichen
> mit unterschiedlicher Bedeutung für Zeitwerte

<!-- --{{0}}-- Idee des Codeblockes
  * Illustration, dass die Formatierungsmethoden auf verschiedenen Ebenen
  funktionen
  * Hinweis auf fehlender Möglichkeit einer Breitenangabe
  * Einführung einer cultural Instanz

 -->
```csharp   WriteDate
using System;
using System.Globalization;
using System.Threading;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
      //Globale Definition des Kulturkreises
      //Thread.CurrentThread.CurrentCulture = new CultureInfo("en-US");
      DateTime thisDate = new DateTime(2008, 3, 15);
      Console.WriteLine(thisDate.ToString("d"));   // d = kurzes Datum
                                                   // D = langes Datum
                                                   // f = vollständig
      Console.WriteLine("{0:D}", thisDate);
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

### Zeichenfolgeninterpolation

Wesentliches Element der Zeichenfolgeninterpolation ist die "Ausführung" von
Code innerhalb der Ausgabespezifikation. Die Breite der Möglichkeiten reicht dabei
von einfachen Variablennamen bis hin zu komplexen Ausdrücken. Hier bitte
Augenmaß im Hinblick auf die Lesbarkeit walten lassen! Angeführt wird ein
solcher Ausdruck durch ein `$`.

```csharp   Zeichenfolgeninterpolation
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
      // Composite formatting:
      var date = DateTime.Now;
      string city = "Freiberg";
      Console.WriteLine("Hello, {0}! Today is {1}, it's {2:HH:mm} now.", city, date.DayOfWeek, date);
      // String interpolation:
      Console.WriteLine($"Hello, {city}! Today is {date.DayOfWeek}, it's {date:HH:mm} now.");
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)


Die Darstellung des Ausdrucks folgt dabei der Semantik:

```
{<interpolatedExpression>[,<alignment>][:<formatString>]}
```

Damit lassen sich dann sehr mächtige Ausdrücke formulieren vgl. [Link](https://docs.microsoft.com/de-de/dotnet/csharp/language-reference/tokens/interpolated) oder aber in den Beispielen von

[Interpolated.cs](https://github.com/liaScript/CsharpCourse/blob/master/code/04_IO_Ausnahmebehandlung/Interplolated.cs)

### Ziele der Schreiboperationen

Üblicherweise möchte man die Ausgabegeräte (Konsole, Dateien, Netzwerk, Drucker,
etc.) anpassen können. `System.IO` bietet dafür bereits verschiedene
Standardschnittstellen.

```csharp
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
       Console.WriteLine("Ausgabe auf das Standard-Gerät");
       Console.Out.WriteLine("Ausgabe nach Out (default die Konsole)");
       //Console.Error.WriteLine("Ausgabe an die Fehlerschnittstelle");
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

Die dafür vorgesehenen Standardeinstellungen können aber entsprechend umgelenkt
werden. Dafür greift C# analog zu vielen anderen Sprachen (und Betriebssysteme)
das Stream Konzept auf, dass eine Abstraktion für verschiedene Quellen und
Senken von Informationen bereitstellt. Darauf aufbauend sind dann Lese- /
Schreiboperationen möglich.

Einen guten Überblick dazu bietet: https://www.youtube.com/watch?v=CN5A3Q2ePak

Von der Klasse `System.IO.Stream` leiten sich entsprechend [MemoryStream](https://docs.microsoft.com/de-de/dotnet/api/system.io.memorystream?view=netframework-4.7.2), [FileStream](https://docs.microsoft.com/de-de/dotnet/api/system.io.filestream?view=netframework-4.7.2), [NetworkStream](https://docs.microsoft.com/de-de/dotnet/api/system.net.sockets.networkstream?view=netframework-4.7.2) ab. Diese bringen sowohl
eigene Lese-/Operationen mit, gleichzeitig ist aber auch das "Umlenken" von
Standardoperationen möglich.

```CSharp
// Das Beispiel entstammt der Dokumentation des .Net Frameworks
// https://docs.microsoft.com/de-de/dotnet/api/system.console.out?view=netframework-4.7.2

using System;
using System.IO;

public class Example
{
   public static void Main()
   {
      // Get all files in the current directory.
      string[] files = Directory.GetFiles(".");
      Array.Sort(files);

      // Display the files to the current output source to the console.
      Console.WriteLine("First display of filenames to the console:");
      Array.ForEach(files, s => Console.Out.WriteLine(s));
      Console.Out.WriteLine();

      // Redirect output to a file named Files.txt and write file list.
      StreamWriter sw = new StreamWriter(@".\Files.txt");
      sw.AutoFlush = true;
      Console.SetOut(sw);
      Console.Out.WriteLine("Display filenames to a file:");
      Array.ForEach(files, s => Console.Out.WriteLine(s));
      Console.Out.WriteLine();

      // Close previous output stream and redirect output to standard output.
      Console.Out.Close();
      sw = new StreamWriter(Console.OpenStandardOutput());
      sw.AutoFlush = true;
      Console.SetOut(sw);

      // Display the files to the current output source to the console.
      Console.Out.WriteLine("Second display of filenames to the console:");
      Array.ForEach(files, s => Console.Out.WriteLine(s));
   }
}
```

Darüber hinaus stehen aber auch spezifische Zugriffsmethoden zum Beispiel für
Dateien zur Verfügung.

```CSharp
using System;
using System.Collections.Generic;
using System.Text;
using System.IO; //Erforderlicher Namespace
namespace Wiki
{
    class Program
    {
        static void Main(string[] args)
        {
            // Unmittelbare Ausgabe in eine Datei
            File.WriteAllText(@"C:\test1.txt", "Sehr einfach");

            // Einlesen des gesamten Inhalts einer Textdatei und
            // Ausgabe auf dem Bildschirm
            string text = File.ReadAllText(@"C:\test1.txt");
            Console.WriteLine(text);
        }
     }
}
```

### Beispiel

In Markdown sind Tabellen nach folgendem Muster aufgebaut:

```
| Column I | Column II | Column III      |
|:---------|:----------|:----------------|
| Peter    | 42        | C-Programmierer |
| Astrid   | 23        | Level Designer  |
```

Geben Sie die Daten bestimmte Fußballvereine in einer Markdown-Tabelle aus.

```csharp   GenerateMarkDownTable
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
      string [] clubs = {"Blau Weiß", "Grün Gelb 1905", "Borussia Tralla Trullas", "Eintracht"};
      int [] punkte = {12, 10, 9, 5};
      int maxlength = 0;
      foreach(string club in clubs)  maxlength =  club.Length < maxlength ? maxlength : club.Length ;
      maxlength += 1;

      string output;
      output  = "| ";
      output += "Verein".PadRight(maxlength, ' ') + "| Punkte |\n";
      output += "|:"+ "".PadRight(maxlength, '-') + "|:-------|\n";
      for (int i = 0; i < clubs.Length; i++){
         output += String.Format("| {0}| {1, -7}|\n", clubs[i].PadRight(maxlength, ' '), punkte[i]);
      }
      Console.WriteLine(output);
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

Welche Annahmen werden implizit bei der Erstellung der Tabelle getroffen? Wo
sehen Sie Verbesserungsbedarf?


## Leseoperationen

                                {{0-1}}
*******************************************************************************

Eine grundlegende Eingabemöglichkeit ist die Übergabe von Parametern beim
Aufruf des Programms von der Kommandozeile.

```csharp      CommandlineParameter.cs
using System;

namespace Rextester
{
  public class Program
  {
    public static int Main(string[] args)
    {
      System.Console.WriteLine("Geben Sie einen Ganzzahlwert und einen String als Argumente ein!");
      if (args.Length == 0)
      {
          System.Console.WriteLine("Offenbar keine Eingabe - Fehler!");
          return 1;
      }
      if (args.Length == 2)  // Erwartete Zahl von Parametern
      {
          long num1 = long.Parse(args[0]);
          long num2 = Convert.ToLong(args[0]);
          long num3;
          long.TryParse(args[0], out num3);
          System.Console.WriteLine($"{num1} {num2} {num3}");

          string text = args[1];
          System.Console.WriteLine($"{text}");
      }
      return 0;
    }
```

Dabei nutzt das obige Beispiel 3 Formen der Interpretation der Daten. In den beiden ersten Fällen ist der Entwickler für das Abfangen der _Exceptions_ verantwortlich. Die letzte Variante kapselt dies intern und gibt die möglicherweise eingetretene Ausnahme über die Rückgabewerte aus. Der Code auf Seiten der Anwendung wird kompakter.

1. `long.Parse` parst die Eingabe in einen ganzzahligen Wert, wirft jedoch eine Ausnahme aus, wenn dies nicht möglich ist, wenn die bereitgestellten Daten nicht numerisch sind.

2. `Convert.ToInt64()` konvertiert die Zeichenkettendaten in einen korrekten echten int64-Wert und wirft eine Ausnahme aus, wenn der Wert nicht konvertiert werden kann.

3. Einen alternativen Weg schlägt `int.TryParse()` ein. Die TryParse-Methode ist wie die `Parse`-Methode, außer dass die `TryParse`-Methode keine Ausnahme auslöst, wenn die Konvertierung fehlschlägt.

Auf die Verwendung der Ausnahmen wird im folgenden Abschnitt eingegangen.

*******************************************************************************

                                {{1-2}}
*******************************************************************************

Leseoperationen von der Console (oder anderen Streams) werden durch zwei Methoden abgebildet:

```
public static int Read ();
public static string ReadLine ();
```

```csharp            ReadFromConsole
using System;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
      char ch;
      int x;
      Console.WriteLine("Print Unicode-Indizes");
      do
        {
          x = Console.Read();   // Lesen eines Zeichens
          ch = Convert.ToChar(x);
          Console.Write("{0}-", x);
          // Hier könnte man jetzt eine Filterung realiseren
        } while (ch != '+');
    }
  }
}
```
``` bash stdin
A0,12,B⺀+

```
@Rextester._eval_(@uid,@CSharp,true,`@input(1)`)

Das Beispiel zeigt sehr schön, wie verschiedene Zeichensätze auf unterschiedlich
lange Codes abgebildet werden. Das chinesische Zeichen, dass vor dem Escape-Zeichen "+" steht generiert einen 2Byte breiten Wert.

*******************************************************************************

## Ausnahmebehandlungen

Die C#-Funktionen zur Ausnahmebehandlung unterstützen bei der Handhabung von
unerwarteten oder außergewöhnlichen Situationen, die beim Ausführen von Programmen auftreten.

```csharp   IndizesBreite
using System;
using System.Globalization;

namespace Rextester
{
  public class Program
  {
    public static void Main(string[] args)
    {
        // Beispiel 1: Zugriff auf das Filesystem eines Rechners aus dem Netz
        System.IO.FileStream file = null;
        //System.IO.FileInfo fileInfo = new System.IO.FileInfo(@"NoPermission.txt");

        // Beispiel 2: Division durch Null
        int a = 0, b = 5;
        //a = b / a;
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

Dabei gelten folgende Regeln für den Umgange mit Exceptions:

+ Wenn für eine spezifische Ausnahme kein Ausnahmehandler existiert, beendet sich das Programm mit einer Fehlermeldung.
+ Alle Ausnahmen sind von `System.Exception` abgeleitet und enthalten detaillierte Informationen über den Fehler, z.B. den Zustand der Aufrufliste und eine Textbeschreibung des Fehlers.
+ Ausnahmen, die innerhalb eines `try`-Blocks auftreten, werden auf einen Ausnahmehandler, der mit dem Schlüsselwort `catch` gekennzeichnet ist, umgeleitet.
+ Ausnahmen werden durch die CLR ausgelöst oder in Software mit dem `throw` Befehl.
+ ein `finally`-Block wird im Anschluss an die Aktivierung eines `catch` Blockes ausgeführt, wenn eine Ausnahme ausgelöst wurde. Hier werden Ressourcen freizugeben, beispielsweise ein Stream geschlossen.
+ Wird kein passender catch-Block gefunden, kommt es zur Behandlung in der Laufzeitumgebung.

```csharp   IndizesBreite
using System;

namespace Rextester
{
  public class Program
  {
    static int Berechnung(int a, int b)
    {
      try
      {
          checked {return a + b;}        // Fall 1
          //return a / b;                // Fall 2
      }
      catch (OverflowException e)
      {
        Console.WriteLine("[ERROR] " + e.Message);
        return -2;
      }
  }

  public static void Main(string[] args)
  {
      try
      {
        Berechnung(int.MaxValue, 1);     // Fall 1
        //Berechnung(2, 0);              // Fall 2
      }
      catch (DivideByZeroException e)
      {
        Console.WriteLine("[ERROR] " + e.Message);
      }
    }
  }
}
```
@LIA.eval(`["main.cs"]`, `mono main.cs`, `mono main.exe`)

### Best Practice

Die folgende Darstellung geht auf die umfangreiche Sammlung von Hinweisen
zum Thema Exceptions unter
https://docs.microsoft.com/de-de/dotnet/standard/exceptions/best-practices-for-exceptions
zurück.

* Differenzieren Sie zwischen Ausnahmevermeidung und Ausnahmebehandlung anhand der erwarteten Häufigkeit und der avisierten "Signalwirkung"

```csharp
// Ausnahmevermeidung
if (conn.State != ConnectionState.Closed)
{
    conn.Close();
}

// Ausnahmebehandlung
try
{
    conn.Close();
}
catch (InvalidOperationException ex)
{
    Console.WriteLine(ex.GetType().FullName);
    Console.WriteLine(ex.Message);
}
```

* Auslösen von Ausnahmen statt Zurückgeben eines Fehlercodes
* Verwenden der vordefinierten .NET-Ausnahmetypen
* Enden von Ausnahmeklassen auf das Wort *Exception*
* Vermeiden Sie unklare Ausgaben für den Fall einer Ausnahmebehandlung, stellen Sie alle Informationen bereit, die für die Analyse des Fehlers nötig sind
* Stellen Sie den Status einer Methode wieder her, die von einer Ausnahmebehandlung betroffen war (Beispiel: Code für Banküberweisungen, Abbuchen von einem Konto und Einzahlung auf ein anderes. Scheitert die zweite Aktion muss auch die erste zurückgefahren werden.)
* Testen Sie Ihre Ausnahmebehandlungsstrategie!

### Beispiel Exception-Handling

Schreiben Sie die Einträge eines Arrays in eine Datei!

Lösung unter [ExceptionHandling.cs](https://github.com/liaScript/CsharpCourse/blob/master/code/04_IO_Ausnahmebehandlung/ExceptionHandling.cs)

| Schritt 1: Welche Fehler können auftreten? Welche Fehler werden durch die Implementierung abgefangen? |
| Schritt 2: Wo sollen die Fehler abgefangen werden?                                                    |
| Schritt 3: Gibt es Prioritäten bei der Abarbeitung?                                                   |
| Schritt 4: Sind abschließende "Arbeiten" notwendig?                                                   |

[Link auf die Dokumentation der StreamWriter Klasse](https://docs.microsoft.com/de-de/dotnet/api/system.io.streamwriter.-ctor?view=netframework-4.7.2#System_IO_StreamWriter__ctor_System_String_)


## Aufgaben

- [ ] Entwickeln Sie ein Programm, dass als Kommandozeilen-Parameter eine Funktionsnamen
und eine Ganzzahl übernimmt und die entsprechende Ausführung realisiert. Als
Funktionen sollen dabei `Square` und `Reciprocal` dienen. Der Aufruf erfolgt
also mit

```bash
mono Calculator Square 7
mono Calculator Reciprocal 9
```
Welche Varianten der Eingaben müssen Sie prüfen? Erproben Sie Ihre Lösung mit einem besonders "böswilligen" Nutzer :-)

```csharp    Calculator.cs
using System;

namespace Program
{
  class MainClass
  {
    static double Square(int num) => num * num;
    static double Reciprocal (int num) => 1f / num;

    static void Main(string[] args)
    {
      bool Error = false;
      double result = 0;
      int num = 1;
      if (args.Length == 2)
      {
        // Hier geht es weiter, welche Fälle müssen Sie bedenken?
        // int.TryParse(args[1], out num) erlaubt ein fehlertolerantes Parsen
        // eines strings
      }
      else Error = true;

      if (Error)
      {
        Console.WriteLine("Please enter a function and a numeric argument.");
        Console.WriteLine("Usage: Square    <int> or\n       Reciprocal <int>");
      }
      else
      {
        Console.WriteLine("{0} Operation on {1} generates {2}", args[0], num, result );
      }
    }
  }
}
```
