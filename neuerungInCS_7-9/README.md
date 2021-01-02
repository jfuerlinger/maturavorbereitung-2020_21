# Neuerungen in C# 7.0-7.3 , C# 8.0, C# 9.0

## C# 7.0-7.3 - Tuples
C# bietet umfangreiche Syntax für Klassen und Strukturen, die verwendet wird, um die Entwurfsabsicht zu erläutern. Manchmal erfordert diese umfangreiche Syntax zusätzliche Arbeit mit minimalem Nutzen. Eine schon bekannte Möglichkeit um komplexe Datentypen an Methoden zu übergeben sind die *DTO*s(data transfer objects). Jedoch sind manchmal sehr einfache Strukturen z.B. mit nur 2 Datenelementen zu übergeben. Dafür wurden die __C# Tupel__ entworfen.

Man kann ein Tupel erstellen, indem Sie jedem Member einen Wert zuweisen und ihnen optional auch semantische Namen geben:
```cs
(string Alpha, string Beta) namedLetters = ("a", "b");
Console.WriteLine($"{namedLetters.Alpha}, {namedLetters.Beta}");
```
In einer Tupelzuweisung können auch die Namen der Felder aur der rechten Seite der Zuweisung angegeben werden:
```cs
var alphabetStart = (Alpha: "a", Beta: "b");
Console.WriteLine($"{alphabetStart.Alpha}, {alphabetStart.Beta}");
```
Das *Entpacken* (Dekonstruieren) eines Tupels, die von einer Methode zurückgegeben werden funktioniert wie folgt. Dazu werden für jeden Wert im Tupel seperate Variablen deklariert. 
```cs
(int max, int min) = Range(numbers);
Console.WriteLine(max);
Console.WriteLine(min);
```
Die Funktion *Entpacken* kann auch mittels Deconstruct-Methode in einer Klasse implementiert werden:
```cs
public class Point
{
    public Point(double x, double y)
        => (X, Y) = (x, y);

    public double X { get; }
    public double Y { get; }

    public void Deconstruct(out double x, out double y) =>
        (x, y) = (X, Y);
}
```
Die Klasse mit den Punkten X und Y kann nun wie folgt verwendet werden:
```cs
var p = new Point(3.14, 2.71);
(double X, double Y) = p;
```
Wenn Sie einen Tupel initialisieren, sind die Variablen, die für die rechte Seite der Zuweisung verwendet werden, oft dieselben, wie die Namen, die Sie den Tupelelementen geben möchten. Die Namen von Tupelelementen können von den Variablen abgeleitet werden, die zum Initialisieren der Tupel verwendet werden:
```cs
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

## C# 7.0-7.3 - Discards
Beim Dekonstruieren eines Tupels oder dem Aufrufen einer Methode mit out-Parametern sind Sie gezwungen, eine Variable zu definieren, deren Wert Sie unter umständen nicht interessiert und die Sie nicht zu verwenden beabsichtigen. C# verfügt jetzt über Unterstützung für "Wegwerfvariablen" (discards). Eine "Wegwerfvariable" ist eine lesegeschützte Variable mit dem Namen _ (dem Unterstrichzeichen).
<br>Sie können der "Wegwerfvariable" alle Werte zuweisen, die Sie verwerfen möchten. Abgesehen von der Zuweisungsanweisung kann die "Wegwerfvariable" nicht im Code verwendet werden.

*Discards* können bei folgenden Beispielen eingesetzt werden:
* Beim Dekonstruieren von Tupeln oder benutzerdefinierten Typen.
* Beim Aufrufen von Methoden mit out-Parametern.
* In einem Musterabgleichsvorgang mit den Anweisungen is und switch.
* Wenn man den Wert einer Zuweisung explizit als *Wegwurf* kennzeichnen möchten.

Das folgende Beispiel definiert eine QueryCityDataForYears-Methode, die ein 6-Tupel zurückgibt, das die Einwohneranzahl für eine Stadt für zwei verschiedene Jahre enthält. Der Methodenaufruf im Beispiel wertet nur die zwei Einwohnerzahlen aus. Die restlichen 4 zurückgegebenen Werte werden als Discard verworfen.

```cs
using System;
using System.Collections.Generic;

public class Example
{
    public static void Main()
    {
        var (_, _, _, pop1, _, pop2) = QueryCityDataForYears("New York City", 1960, 2010);

        Console.WriteLine($"Population change, 1960 to 2010: {pop2 - pop1:N0}");
    }

    private static (string, double, int, int, int, int) QueryCityDataForYears(string name, int year1, int year2)
    {
        int population1 = 0, population2 = 0;
        double area = 0;

        if (name == "New York City")
        {
            area = 468.48;
            if (year1 == 1960)
            {
                population1 = 7781984;
            }
            if (year2 == 2010)
            {
                population2 = 8175133;
            }
            return (name, area, year1, population1, year2, population2);
        }

        return ("", 0, 0, 0, 0, 0);
    }
}
// The example displays the following output:
//      Population change, 1960 to 2010: 393,149
```


## C# 7.0-7.3 - Pattern Matching



## C# 7.0-7.3 - Local Functions

___
___
## C# 8.0 - readonly Member


## C# 8.0 - Nullable Reference Types


## C# 8.0 - Asynchronous Streams
___
___
## C# 9.0 - Records

## C# 9.0 - Init only setters

## C# 9.0 - New features for partial methods







[Zur Übersicht](../README.md)




