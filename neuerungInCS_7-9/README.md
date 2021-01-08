# Neuerungen in C# 7.0-7.3 , C# 8.0, C# 9.0
[Zur Übersicht](../README.md)

## C# 7.0-7.3 - Tuples
C# bietet umfangreiche Syntax für Klassen und Strukturen, um die Entwurfsabsicht zu erläutern. Manchmal erfordert diese umfangreiche Syntax zusätzliche Arbeit mit minimalem Nutzen. 
<br>Eine schon bekannte Möglichkeit um komplexe Datentypen an Methoden zu übergeben sind die `DTOs`(data transfer objects). Häufig werden nur sehr einfache Strukturen z.B. mit nur 2 Datenelementen benötigt. Dafür wurden die `C# Tupel` entworfen.

Man kann ein Tupel erstellen, indem Sie jedem Member einen Wert zuweisen und ihnen optional auch semantische Namen geben:
```cs
(string Alpha, string Beta) namedLetters = ("a", "b");
Console.WriteLine($"{namedLetters.Alpha}, {namedLetters.Beta}");
```
In einer Tupelzuweisung können auch die Namen der Felder auf der rechten Seite angegeben werden:
```cs
var alphabetStart = (Alpha: "a", Beta: "b");
Console.WriteLine($"{alphabetStart.Alpha}, {alphabetStart.Beta}");
```
Das `Entpacken` (Dekonstruieren) eines von einer Methode zurückgegebenen Tupels, funktioniert wie folgt. Dazu werden für jeden Wert im Tupel seperate Variablen deklariert. 
```cs
(int max, int min) = Range(numbers);
Console.WriteLine(max);
Console.WriteLine(min);
```
Die Funktion `Entpacken` kann auch mittels `Deconstruct`-Methode in einer Klasse implementiert werden:
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
Wenn Sie einen Tupel initialisieren, sind die Variablen, die für die rechte Seite der Zuweisung verwendet werden, oft dieselben, wie die Namen, die Sie den Tupelelementen geben möchten. Die Namen von den Tupelelementen können von den Variablen der Zuweisung abgeleitet werden:
```cs
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

## C# 7.0-7.3 - Discards
Beim Dekonstruieren eines Tupels oder dem Aufrufen einer Methode mit out-Parametern ist man gezwungen eine Variable zu definieren. Möglicherweise ist der Wert nicht interessant und man beabsichtigt die Variable nicht weiter zu verwenden. C# verfügt jetzt über eine Unterstützung für "Wegwerfvariablen" (`Discards`). Eine "Wegwerfvariable" ist eine lesegeschützte Variable mit dem Namen `_` (dem Unterstrichzeichen).
<br>Abgesehen von der Zuweisungsanweisung kann die "Wegwerfvariable" nicht im Code verwendet werden.

*Discards* können in folgenden Szenarien eingesetzt werden:
* Beim Dekonstruieren von Tupeln oder benutzerdefinierten Typen.
* Beim Aufrufen von Methoden mit out-Parametern.
* In einem Musterabgleichsvorgang mit den Anweisungen is und switch.
* Wenn man den Wert einer Zuweisung explizit als "Wegwurf" kennzeichnen möchten.

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
Pattern Matching bietet mehrere Möglichkeiten um den Code lesbarer zu machen. Damit können Variablen nach Typ, Werten oder Werten mit Eigenschaften geprüft werden.
<br>Pattern Matching unterstützt `is`-Ausdrücke und `switch`-Ausdrücke. Beide ermöglichen das Überprüfen eines Objekts auf dessen Eigenschaften, um zu bestimmen ob das Objekt dem gesuchten Muster entspricht. Das `when`-Schlüsselwort kann verwendet werden, um zusätzliche Regeln für das Muster anzugeben.

Der folgende Code überprüft, ob es sich bei der Variable um einen `int`-Wert handelt und fügt sie, wenn dies der Fall ist, der aktuellen Summe hinzu:
```cs
if (input is int count)
    sum += count;
````

Der aktuelle `switch`-Ausdruck verfügt über mehrere neue Konstrukte:
* Der `switch` Ausdruck ist nicht mehr beschränkt auf ganzzahlige Typen, Enum, string oder Nullable-Typ. Es kann nun jeder Typ verwendet werden.
* Man kann den Wert, wie schon beim `is`-Ausdruck, einer neuen Variable zuweisen
* Wenn man zusätzliche Prüfungen an die Bedingung anfügen möchte, kann man dies mit der `when`-Klausel tun.  

Im folgenden Code werden diese Funktionen veranschaulicht:
```cs
public static int SumPositiveNumbers(IEnumerable<object> sequence)
{
    int sum = 0;
    foreach (var i in sequence)
    {
        switch (i)
        {
            case 0:
                break;
            case IEnumerable<int> childSequence:
            {
                foreach(var item in childSequence)
                    sum += (item > 0) ? item : 0;
                break;
            }
            case int n when n > 0:
                sum += n;
                break;
            case null:
                throw new NullReferenceException("Null found in sequence");
            default:
                throw new InvalidOperationException("Unrecognized type");
        }
    }
    return sum;
}
```
* `case 0`: ist das bereits bekannte Konstanten-Muster.
* `case IEnumerable<int> childSequence`: ist ein Typ-Muster.
* `case int n when n > 0`: ist ein Typ-Muster mit einer zusätzlichen `when`-Bedingung.
* `case null`: ist das NULL-Muster.
* `default`: ist der bereits bekannte default-Zweig.


## C# 7.0-7.3 - Local Functions
Man kann nun Funktionen in Funktionen verschachteln und dadurch den Aufrufbereich und die Sichtbarkeit einschränken.

Zwei häufige Anwendungsfälle für lokale Funktionen sind öffentliche Iteratormethoden und öffentliche asynchorne Methoden. Beide Methoden haben die Problematik, dass Fehler, die in den Methoden auftreten, sich erst zu einem späteren Zeitpunkt auswirken können. Dies erschwert die Fehlersuche. Daher sind beim Aufruf der Methode besonders genaue Parameterüberprüfungen nötig.
Im folgenden Beispiel wird gezeigt, wie man die Parameter-Validierung mithilfe einer lokalen Funktion von der `Iterator`-Implementierung trennen kann:
```cs
public static IEnumerable<char> AlphabetSubset3(char start, char end)
{
    if (start < 'a' || start > 'z')
        throw new ArgumentOutOfRangeException(paramName: nameof(start), message: "start must be a letter");
    if (end < 'a' || end > 'z')
        throw new ArgumentOutOfRangeException(paramName: nameof(end), message: "end must be a letter");

    if (end <= start)
        throw new ArgumentException($"{nameof(end)} must be greater than {nameof(start)}");

    return alphabetSubsetImplementation();

    IEnumerable<char> alphabetSubsetImplementation()
    {
        for (var c = start; c < end; c++)
            yield return c;
    }
}
```
Das gleiche Verfahren kann für `Async`-Methoden eingesetzt werden, um sicherzustellen, dass mögliche Fehler mittels Argumentüberprüfung abgefangen werden, bevor der asynchrone Aufruf beginnt:
```cs
public Task<string> PerformLongRunningWork(string address, int index, string name)
{
    if (string.IsNullOrWhiteSpace(address))
        throw new ArgumentException(message: "An address is required", paramName: nameof(address));
    if (index < 0)
        throw new ArgumentOutOfRangeException(paramName: nameof(index), message: "The index must be non-negative");
    if (string.IsNullOrWhiteSpace(name))
        throw new ArgumentException(message: "You must supply a name", paramName: nameof(name));

    return longRunningWorkImplementation();

    async Task<string> longRunningWorkImplementation()
    {
        var interimResult = await FirstWork(address);
        var secondResult = await SecondStep(index, name);
        return $"The results are {interimResult} and {secondResult}. Enjoy.";
    }
}
```
___
## C# 8.0 - Readonly Member
Der `readonly`-Modifier kann auf Member der `Strukturen` angewendet werden. Es zeigt an, dass der Status nicht geändert werden kann. Es ergeben sich damit granularere Möglichkeiten die Zugriffe zu beschränken.
Hier ein Beispiel für die Handhabung mit folgender Struktur:
```cs
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```
Wie bei den meisten Strukturen ist die ToString()-Methode nicht änderbar. Man kann dies durch Hinzufügen des `readonly`-Modifikators bei der Deklaration von ToString() angeben:
```cs
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```
Die vorhergehende Änderung generiert nun eine Compilerwarnung, weil ToString auf das `Distance`-Property zugreift, die nicht als readonly markiert ist:
```
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```
Die Warnung kann behoben werden, indem in der Deklaration für `Distance` der readonly-Modifier hinzugefügt wird:
```cs
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```
Der `readonly`-Modifier wird bei einem schreibgeschützten Property benötigt. Der Compiler geht nicht davon aus, das reine `get`-Zugriffsmethoden den Zustand nicht ändern. Eine Ausnahme bilden die `autogenerated` Properties. Der Compiler behandelt alle `autogenerated` Properties ohne `set` als `readonly`.

Mit diesem Feature kann man die Designabsicht angeben. Damit erzwingt der Compiler, basierend auf dieser Absicht, die entsprechende Implementierung.
Beispiel: Die folgende Methode wird nicht kompiliert, es sei denn, man entfernt den readonly-Modifizierer.
```cs
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```
## C# 8.0 - Nullable Reference Types
### Aktivierung
Die neue `Nullable` Notation kann für das gesamte Projekt in der CSPROJ-Datei freigegeben werden:
```xml
<Nullable>enable</Nullable>
```
Oder abschnittsweise im Code als Anweisung durch `#nullable enable` aktiviert bzw. `#nullable disable` deaktiviert werden.

### Merkmale:
Mit der gleichen Syntax wie für die `Valuetypes` kann nun auch ein `Referencetype` als `Nullable` deklariert werden. Es wird ein `?` am Variablentyp angehängt.
Z.B.:
```cs
string? name;
```

Der Compiler führt innerhalb der `nullable` Notation, je nach Referencetyp, unterschiedliche Überprüfungen durch. Es werden Warnungen bei der Kompilierung ausgegeben. Das Programm kann trotz Warnungen ausgeführt werden. Zur Laufzeit sind `Nullable-Referencetypes` und `Non-Nullable-Referencetypes` gleichwertig.  

#### `Non-Nullable-Referencetype`
Für diesen Variablentyp erwingt der Compiler Regeln, damit das Dereferenzieren ohne vorherige `NULL` Überprüfung sicher ist:
1. Die Variable muss mit einem Wert ungleich `NULL` initialisiet werden
2. Der Variable kann nie der Wert `NULL` zugewiesen werden.

Werden die Regeln missachtet, werden folgende Warnungen ausgegeben:
```
warning CS8618: Non-nullable property 'NonNullableProperty' must contain a non-null value when exiting constructor. Consider declaring the property as nullable.
warning CS8600: Converting null literal or possible null value to non-nullable type
```
#### `Nullable-Referencetype`
Dieser Datentyp darf null sein. Damit überprüft der Compiler ob vor der Dereferenzierung auf `!= Null` überprüft wurde. Eine `Null` Zuweisung ist bei diesem Typ erlaubt.
Fehlt im Code die Not-Null-Überprüfung, gibt der Compiler folgende Warnung aus:
```
warning CS8602: Dereference of a possibly null reference
```
## C# 8.0 - Asynchronous Streams
Eine Methode die einen asynchronen Stream zurückliefert hat 3 Eigenschaften:
1. Sie ist mit `async` deklariert
2. Sie gibt einen `IAsyncEnumerable<T>` zurück
3. Die Methode beinhaltet `yield return` statements um aufeinanderfolgende Elemente als asynchronen Stream zurückzugeben.

Der folgende Code generiert eine Reihe von Zahlen von 0 bis 19, mit einer Wartezeit von 100ms zwischen den Generierungsvorgängen:
```cs
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```
Um den asynchronen Stream abzurufen, muss das `await` Schlüsselwort beim `foreach` vorangestellt werden:
```cs
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```
Anwendungsgebiete:
* Zyklische HTTP-calls mit Verarbeitung der Empfangsdaten.
    ```cs
    await foreach(var someValue from someAsyncIterator(5))
    {
        ...
    }

    IAsyncEnumerable<string> someAsyncIterator(int max)
    {
        for(int i=0;i<max;i++)
        {
            var response=await httpClient.GetStringAsync($"{baseUrl}/{i}");
            yield return response;
        }
    }
    ```
___
## C# 9.0 - Records
### Anforderung
Man benötigt das Targetframework `.NET5` und den C#9.0 Compiler. Der C#9.0 Compiler ist ab `Visual Studio 2019 version 16.8` verfügbar.
Im Projektfile muss das Targetframework auf .NET5 eingestellt werden:
```cs
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```
### Eigenschaften
 Record types sind `Reference Types` (wie Klassen) mit erweiterter Funktionalität. Sie sind in ihrer Standardeinstellung `readonly` und somit `immutable`. Das bedeutet, dass nach der Initialisierung die Werte der Properties sich nicht mehr ändern können. Der `Record` ist dadurch Thread-Safe. Die `immutable` Eigenschaft stellt sicher, dass kein Thread die Werte im Record verändern kann. Weiters haben sie durch die angesprochende Funktionserweiterung, bezogen auf die Vergleichsfunktion, ein ähnliches Verhalten wie `Value Types`.

Records werden mit dem Schlüsselwort `record` deklariert. Der folgende Record beinhaltet 2 Properties die automatisch dem Konstruktor entnommmen werden. Die Properties sind `public` und `readonly`.
```cs
// a Record is a "class" with additional functions
// Immutable - The values cannot be changes
public record Person(string FirstName, string LastName);
```
Das entspricht folgender Klassendefinition. Die Properties können nur beim Erstellen der Klasse gesetzt werden. Daher `init` an der Stelle `set`. Verglichen mit der Schreibweise des `Records` ist die Definition der Klasse wesentlich länger:
```cs
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }

    public Person(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }
}
```
Analog zu den Klasse können `Records` durch Properties und Methoden erweitert werden. Die Sichtbarkeit der Properties z.B. internal kann auch eingeschränkt werden:
```cs
public record Person(string FirstName, string LastName)
{
    internal string FirstName { get; init; } = FirstName;
    public string FullName { get => $"{ FirstName } { LastName }"; }

    public string SayHello()
    {
        return $"Hello, my name is { FirstName }";
    }
}
``` 
Der `Record type` stellt automatisch noch weitere Funktionen zur Verfügung:
* Überladung der Operatoren `==` und `!=`. Es werden die Werte der Properties verglichen. Nicht die Adresse, wie bei den `Reference Types` üblich!
* Die Methode `Equals` vergleicht die Werte der Properties. 
* Überladung der Methode `GetHashCode()`. Der Hashcode wird nur anhand der Werte generiert. Die Speicheradresse wird nicht (wie bei Klassen) miteinbezogen. 2 Records mit dem selben Inhalt liefern den gleichen Hashcode.
* Records beinhalten eine `copy` und `clone` Funktionalität
* Die `ToString` Methode liefert die Properties mit Namen und Werte in `{}` Klammern
* Die Methode `PrintMembers` liefert die Properties mit Namen und Werte ohne Klammern
* Der Compiler erstellt automatisch die `Destruct` Methode

Ein Beispiel für den Vergleich. `Person` ist vom Typ `Record`:
```cs
var person1 = new Person("Bill", "Wagner");
var person2 = new Person("Bill", "Wagner");

Console.WriteLine(person1 == person2); // true
```
Das Kopieren funktioniert mit Hilfe des `with` Ausdrucks. In dem nachfolgenden Beispiel wird der Nachname von dem Original übernommen und der Vorname durch Paul ersetzt.
```cs
Person brother = person with { FirstName = "Paul" };
```

Lässt man die {} Klammern leer entsteht eine 1:1 Kopie:
```cs
Person clone = person with { };
```
Man kann auch Records voneinander ableiten. Eine Mischung von Klassen und Records ist nicht möglich!
```cs
public record Pet(string Name)
{
    public void ShredTheFurniture() =>
        Console.WriteLine("Shredding furniture");
}

public record Dog(string Name) : Pet(Name)
{
    public void WagTail() =>
        Console.WriteLine("It's tail wagging time");

    public override string ToString()
    {
        StringBuilder s = new();
        base.PrintMembers(s);
        return $"{s.ToString()} is a dog";
    }
}
```
Mithilfe der `Destruct` Methode werden alle `public` Properties getrennt nach aussen übergeben:
```cs
var person = new Person("Bill", "Wagner");

var (first, last) = person;
Console.WriteLine(first);
Console.WriteLine(last);
```
### Vorteile
* Vereinfachte Schreibweise
* Thread-Safe => `immutable`
* Sicherheit bei Übergabe an Methoden (Der Datensatz kann innerhalb der Methode nicht unbeabsichtigt verändert werden)  => `immutable`

### Verwendung
* Empfang von Daten die sich nicht ändern (z.B. WetterService)
* API Aufrufe
* Verwaltung von Prozessdaten
* Verwaltung von ReadOnly Daten
### Don´ts 
Records sollen immer als readonly verwendet werden. Das nachfolgende Beispiel funktioniert, ist allerdings ein schlechter Programmierstil:
```cs
public record Person // No constructor so no deconstructor
{
    public string FirstName { get; set; } // The set makes this record mutable (BAD!)
    public string LastName { get; set; } // The set makes this record mutable (BAD!)
}
```
## C# 9.0 - Init only setters
__Init only setters__ unterstützen eine konsistente Schreibweise für das Initialisieren von Properties in einem Objekt. Es kann nun die `init` Zugriffsberechtigung anstelle der `set` Berechtigung vergeben werden. Damit kann das Property nur mehr in einem bestimmten Zeitfenster gesetzt werden. Das Zeitfenster endet mit Abschluss der Konstruktionsphase des Objekts. Diese beinhaltet die Konstruktoraufrufe, den Objektinitialisierer und die `with` Operation.

Beispiel:
```cs
public struct WeatherObservation
{
    public DateTime RecordedAt { get; init; }
    public double TemperatureInCelsius { get; init; }
    public double PressureInMillibars { get; init; }
}
```
Aufrufer können den Wert mit der Objektinitialisierung noch verändern:
```cs
var now = new WeatherObservation 
{ 
    RecordedAt = DateTime.Now, 
    TemperatureInCelsius = 20, 
    PressureInMillibars = 998.0 
}
```
Die Änderung nach der Initialisierung führt jedoch zu einem Fehler:
```cs
// Error! CS8852.
now.TemperatureInCelsius = 18;
```
## C# 9.0 - New features for partial methods
In den vorangegangenen Versionen gab es folgende Einschränkungen für die `extend partial methods`:
* Der Returntype muss vom Typ `void` sein
* Sie können keinen `out` Parameter haben
* Der Zugriffsmodifier muss `private` sein

Diese Einschränkungen mussten für den neuen [C# code generator](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9?WT.mc_id=DOP-MVP-5003880#support-for-code-generators) entfernt werden.

Folgendes Beispiel kombiniert alle pre-C#9.0 Einschränkungen und zeigt wie es nun sinnvoll eingesetzt werden kann:
```cs
partial class Parser
{
    internal partial bool TryParse(string s, out int i);
}

partial class Parser
{
    internal partial bool TryParse(string s, out int i)
    {
        i = 0;
        return true;
    }
}
```
___

Weiterführende Informationen zu diesem Thema finden Sie auf der [Microsoft-Webseite](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-9)

[Zur Übersicht](../README.md)




