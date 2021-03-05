# LINQ
[Zur Übersicht](../README.md)

## Einführung
**L**anguage-**In**tegrated **Q**uery (LINQ) ist eine Kurzform für sprachintegrierten Abfrage und wird hauptsächlich zum Abfragen von Daten (Speichern und Abrufen) aus einer anderen Datenquelle wie SQL-Datenbank, JSON und XML-Dokumenten, ADO.Net-Dataset, HTTP-basierte Informationsdienste und .NET-Sammlungen verwendet.
### Datenbankzugriff unter LINQ
Ein zentraler LINQ Aspekt ist, das LINQ die Zusammenarbeit mit beliebigen Typen von Objekten oder Datenquellen ermöglicht und dazu ein konsistentes Programmiermodell bereitstellt. 
#### Wichtige LINQ-Eigenschaften:
- Erweiterung der Programmiersprachen wie Visual Basic und C#
- Ist streng typisiert
- Abfragen werden zur Entwurfszeit geprüft, nicht erst zur Laufzeit
- „Intellisense“-Unterstützung in Visual Studio
- LINQ bietet allgemeine Abfragen für verschiedene Arten von Datenquellen wie SQL-Datenbanken, ADO.NET-Datensätze, XML-Dokumente, .NET-Sammlung usw.
- LINQ reduziert die Anzahl der Codierungszeilen. Dutzende von Codezeilen können in einer einzigen Zeile in LINQ geschrieben werden.
- LINQ ist ein leicht verständlicher und besser lesbarer Code. So können andere Entwickler die LINQ-Abfrage leicht verstehen.
#### Die Funktionsweise:
+ LINQ-Ausdrücke werden vom Compiler in Extension Methods (Erweiterungsmethoden) und Lambda Expressions übersetzt
+ Durch die Lambda Expressions steht der LINQ-Ausdruck zur Laufzeit als Expression Tree (Abfragebaum) zur Verfügung
+ Die Expression Trees werden je nach LINQ-Variante in eine andere Darstellung (wie z.B. SQL) übersetzt
+ In LINQ arbeiten wir immer mit Objekten, sodass dasselbe grundlegende Codierungsmuster mit verschiedenen Arten von Datenquellen verwenden können.  

Der *Expression Tree* ist eine Darstellung eines Lambda-Ausdrucks in dem internen Speicher. Es enthält die tatsächlichen Elemente der Abfrage, nicht das Ergebnis der Abfrage. Wir können mit den Daten im Ausdrucksbaum wie mit jeder anderen Datenstruktur interagieren. Egal wie einfach oder komplex sie sind, Abfrageausdrücke sind einfach spezialisierter Syntax für Methodenaufrufe.  
Da IQueryable <T> von IEnumerable <T> abgeleitet ist, ist Verwendung von LINQ to Objects-Operatoren für eine beliebige EntityFramework-Quelle möglich. 
## LINQ Sprachmerkmale
* Typinferenz
* Erweiterungsmethoden
* Lambda-Ausdrücke
* Anonyme Typen
* Objekt- und Collectioninitialisierer  
### Typinferenz
Unter Typinferenz versteht man ein Sprachmerkmal welches es erlaubt, dass der Datentyp lokaler Variablen bei der Deklaration vom Compiler automatisch ermittelt wird, ohne dass explizit der Typ angegeben werden muss. Hier kommt ins Spiel der Schlüsselwort „var“.
#### Beispiel:
Die Initialisierung der Variablen a wird vom Compiler ausgewertet und der Typ aufgrund des Wertes 35 auf Integer festgelegt.  
`var a = 35;` ist semantisch identisch mit `int a = 35;`  
Das „var“ ist aber auch nützlich, wenn man den Typ eines soeben erzeugten Objekts nicht in der Deklaration wiederholen will. Statt  
`Dictionary<string, int> dict = new Dictionary<string, int>();`  
kann man einfach schreiben  
`var dict = new Dictionary<string, int>();`
### Lambda-Ausdruck
Ein Lambda-Ausdruck ist eine anonyme Funktion, von der ein einzelner Wert berechnet und zurückgegeben wird. Im Gegensatz zu benannten Funktionen kann ein Lambda-Ausdruck gleichzeitig definiert und ausgeführt werden. Der Rückgabetyp wird durch den Typ des rechtsstehenden Ausdrucks bestimmt. 
#### Beispiel:
```csharp
public Delegate double MPlyDelegate (double x, double y);   
MPlyDelegate mply = (double x, double y) => x * y;
string test = mply(5.5, 4.3).ToString();
```  
// Methodenaufruf: test = 23.65  

Anzahl der Parameter und deren jeweiliger Datentyp mit denen des Delegaten, für den
der Lambda-Ausdruck angegeben wird, müssen übereinstimmen.  
Lambda-Ausdrücke werden oft als ***Prädikate*** verwendet, d.h. als Funktionen, die eine
bestimmte Bedingung prüfen und als Ergebnis true oder false zurückgeben.
Der Delegate-Typ  
`delegate bool Predicate<T> (T val);`  
beschreibt zum Beispiel alle Methoden, die einen Wert eines beliebigen Typs T als
Parameter bekommen, irgendeine Eigenschaft dieses Werts prüfen und true oder false
zurückliefern.  
```csharp
//Get the average of the odd Fibonacci numbers in the series... 

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace lambdaexample {
   class Program {     
      static void Main(string[] args)
      {
         int[] fibNum = { 1, 1, 2, 3, 5, 8, 13, 21, 34 };
         double averageValue = fibNum.Where(num ⇒ num % 2 == 1).Average();
         Console.WriteLine(averageValue);
         Console.ReadLine();
      }
   }
}
```  
### Objekt-Initialisierer
Durch Objekt-Initialisierer wird es möglich, ähnlich wie bei der Initialisierung von Attributen, Felder und Eigenschaften einer Klasse (oder Struktur) auf Anfangswerte zu setzen.  
#### Beispiel:
```csharp
public class Customer 
{   
   string Name; 
   int PLZ;
   string Ort;
}
```

Das Erzeugen und Initialisieren einer Instanz von Customer bedarf keines speziellen Konstruktors:  
```csharp
var kunde = new Customer
{
   Name = Max,
   PLZ = 4600,    
   Ort = Wels 
};
```  

Man braucht Objektinitialisierer vor allem in Lambda-Ausdrücken, wo ein Parameter auf einen Rückgabewert abgebildet wird, wie etwa in:  
`IEnumerable<Person> persons = list.Select(x => new Person {Id = x});`  
### Anonyme Typen
Darunter verstehen wir einfache namenlose Klassen, die vom Compiler automatisch erzeugt werden und die nur über Eigenschaften und dazugehörige private Felder verfügen. »Namenlos« bedeutet, dass uns der Name der Klasse nicht bekannt ist und wir deshalb keinen direkten Zugriff auf die Klasse haben. Lediglich eine Instanz steht zur Verfügung, die man ausschließlich lokal, d.h. im Bereich der Deklaration, verwenden kann.  
#### Beispiel:
Der Ausdruck  
`new { Length = 100, Width = 50 }`  
erzeugt ein Objekt der folgenden anonymen Klasse

```csharp
class ??? 
{
   public int Length { get; private set; }  
   public int Width { get; private set; }  
}
```  
und initialisiert Length mit 100 und Width mit 50.  
Bei der Erzeugung von Objekten anonymer Klassen gelten folgende Regeln:
-	Die Properties einer anonymen Klasse sind read-only,
-	Man kann die Properties einer anonymen Klasse entweder explizit benennen:  
`new { Name = “Max Musterman” }`  
oder implizit  
`new { student.Name }`,  
indem man sie aus den Namen anderer Felder, Properties oder lokaler Variablen ableitet. Explizite und
implizite Benennung können sogar gemischt werden:  
`new { student.Name, Id = 91 })`
-	Anonyme Klassen haben zwar keinen Namen, sind aber mit Object kompatibel,
-	Zusätzlich zu den Properties erzeugt der Compiler für jede anonyme Klasse eine
Implementierung der Methode ToString(). Der Aufruf  
`Console.WriteLine( new { student.Name, Id = 91 } );`  
liefert zum Beispiel die Ausgabe  
`{ Name = Max Musterman, Id = 91 }`  
### Erweiterungsmethoden
Normalerweise erlaubt eine objektorientierte Programmiersprache das Erweitern von Klassen durch Vererbung. Mit Erweiterungsmethoden können wir einem Datentyp oder einer Schnittstelle Methoden außerhalb der Definition hinzufügen.
Eine Erweiterungsmethode ist eine Methode, die einen existierenden Typ (eine Klasse, einen Struct oder ein Interface) um neue Funktionalität erweitert, ohne in diesem Typ deklariert zu sein. Sie wird als statische Methode einer beliebigen statischen Klasse deklariert und ordnet sie dem fremden Typ über den Typ ihres ersten Parameters zu.  
#### Beispiel
Wenn mann einer Klasse Kunde eine Erweiterungsmethode Orders zuordnen will, so muss der erste Parameter von Orders vom Typ Customer sein und mit dem Schlüsselwort ***„this“*** gekennzeichnet sein, also:  
`static void Orders(this Customer par, ...) { … }`  
Wenn man nun eine Variable _“kunde“_ vom Typ _Customer_ hat, kann man die Methode _Orders_ wie eine
Methode von _Customer_ aufrufen, also:  
`kunde.Orders(…);`  
### Query-Ausdrücke
Es gibt zwei alternative Schreibweisen von LINQ Abfragen:
-	Query Expression-Syntax (Abfrage-Syntax)
-	Extension Method-Syntax (Erweiterungsmethoden-Syntax)  
Da diese Erweiterungsmethoden in der Bibliotheksklasse _System.Linq.Enumerable_ deklariert sind,
müssen wir den _Namensraum System.Linq_ importieren:  
> using System.Linq;  
#### Beispiel:  
```csharp
var result = from s in students  
   where s.Field == “Computing”  
   select new { s.Name, s.Id };  
```
```csharp
var result = students  
  .Where(s => s.Field == “Computing”)  
  .Select(s => new { s.Name, s.Id });  
```
Die Query Expression-Syntax (Abfragesyntax) ermöglicht das Schreiben von Abfragen in einer SQL-ähnlichen Weise. Allerdings unterstützt die Query Expression-Syntax nicht jeden standardmäßigen Abfrageoperator.  
### LINQ Operatoren  
LINQ-Operatoren sind keine Operatoren im üblichen C# -Sinne wie __+__ oder __&&__. LINQ hat eine eigene Terminologie. Ein LINQ-Operator ist nichts weiter als eine Methode, die einem Standardmuster entspricht.
#### Übersicht der wichtigsten Abfrageoperatoren  
||||
|-----------------------------|:-------------:|---------------------------------:|  
|Beschränkungsoperatoren|(Restriction)|Where|  
|Projektionsoperatoren|(Projection)|Select, SelectMany|  
|Sortieroperatoren|(Ordering)|OrderBy, ThenBy|  
|Gruppierungsoperatoren|(Grouping)|GroupBy|  
|Quantifizierungsoperatoren|(Quantifiers)|Any, All, Contains|  
|Aufteilungsoperatoren|(Partitioning)|Take, Skip, TakeWhile, SkipWhile|  
|Mengenoperatoren|(Sets)|Distinct, Union, Intersect, Except|  
|Elementoperatoren|(Elements)|First, FirstOrDefault, ElementAt|  
||||  
#### __Der Restriktionsoperator „Where“__  
Dieser Operator schränkt die Ergebnismenge anhand einer Bedingung ein. 
Außerdem können auch Indexparameter verwendet werden, um die Filterung auf bestimmte Indexpositionen zu begrenzen.  
#### __Die Projektionsoperatoren „Select“ und „SelectMany“__  
Diese Operatoren »projizieren« die Inhalte einer Quell-Auflistung in eine Ziel-Auflistung, die das Abfrageergebnis repräsentiert.  
*__Select__*  
Der Operator macht die Abfrageergebnisse über ein Objekt verfügbar, welches *IEnumerable(Of T)* implementiert.
Bei einem Zugriff auf eine Datenbank, kann der Operator *Select*, die Menge der abgerufenen Daten reduzieren, wodurch die Last auf dem Datenbank Server verringert werden kann.  
*__SelectMany__*  
Der *SelectMany* LINQ-Operator wird in Abfrageausdrücken verwendet, die mehrere Ausgabeelements zurückliefern.  
```csharp
internal class Student
{
    public int ID { get; set; }
    public string Name { get; set; }
}
 
class Program
{
    static void Main(string[] args)
    {
        List<Student> students = new List<Student>();
        students.Add(new Student
            {
                ID = 1,
                Name = "Michael"
            });
 
        students.Add(new Student
            {
                ID = 2,
                Name = "Nico"
            });
 
        students.Add(new Student
            {
                ID = 3,
                Name = "Zvonko"
            });
 
        var result = students.Where(w => w.Name.Contains("N")).Select(w => w.Name);
 
        Console.WriteLine(result.First()); // Print Nico
    }
}
```
#### __Die Sortierungsoperatoren „OrderBy“ und „ThenBy“__  
Diese Operatoren bewirken ein Sortieren der Elemente innerhalb der Ergebnismenge.  
__OrderBy/OrderByDescending__  
Im Allgemeinen garantieren LINQ-Abfragen nicht, dass Elemente in einer bestimmten Reihenfolge zurückgeliefert werden es sei denn, wir definieren explizit die gewünschte Reihenfolge. Dies geschieht in einem Abfrageausdruck
mit einer „OrderBy“-Klausel.  
__ThenBy/ThenByDescending__  
Diese Operatoren verwendet man, wenn nacheinander nach mehreren Schlüsseln sortiert werden soll.  
```csharp
List<Student> students = new List<Student>();
students.Add(new Student { Id = 1, Name = "Rudi", Rank = 1, Age = 39 });
students.Add(new Student { Id = 2, Name = "Kurt", Rank = 1, Age = 32 });
students.Add(new Student { Id = 3, Name = "Siegfried", Rank = 2, Age = 45 });
students.Add(new Student { Id = 4, Name = "Mario", Rank = 2, Age = 39 });
 
var studentsOrderByRank = students.OrderBy(w => w.Rank).ThenBy(w => w.Age);
 
Console.WriteLine("Sortierte Studenten:");
foreach (var student in studentsOrderByRank)
{
    Console.WriteLine(student.Name);
}
```  
Sortierte Studenten:
> Kurt  
> Rudi  
> Mario  
> Siegfried  
#### __Der Gruppierungsoperator GroupBy__  
Dieser Operator kommt dann zum Einsatz, wenn das Abfrageergebnis in gruppierter Form zur Verfügung stehen soll.
*GroupBy* wählt die gewünschten Schlüssel-Elemente-Zuordnungen aus der abzufragenden Auflistung aus.  
#### Beispiel:  
Zuerst werden 2 Klassen definiert:
```csharp
	public class Wagen
	{
		public int Length { get; set; }
	}

	public class Zug
	{
		public string Country { get; set; }
		public List<Wagen> Wagens { get; set; }
	}
```  
```csharp
static void Main(string[] args)
{
    List<Zug> zugs = new List<Zug>();
    zugs.Add(new Zug()
    {
        Country = "österreich",
        Wagens = new List<Wagen>() {
            new Wagen() { Length = 25 },
            new Wagen() { Length = 25 } }
    });
    zugs.Add(new Zug()
    {
        Country = "Österreich",
        Wagens = new List<Wagen>() {
            new Wagen() { Length = 20 },
            new Wagen() { Length = 20 },
            new Wagen() { Length = 18 } }
    });
    zugs.Add(new Zug()
    {
        Country = "Deutschland",
        Wagens = new List<Wagen>() {
            new Wagen() { Length = 22 },
            new Wagen() { Length = 22 },
            new Wagen() { Length = 19 },
            new Wagen() { Length = 19 } }
    });
    zugs.Add(new Zug()
    {
        Country = "deutschland",
        Wagens = new List<Wagen>() {
            new Wagen() { Length = 22 },
            new Wagen() { Length = 22 },
            new Wagen() { Length = 19 },
            new Wagen() { Length = 19 },
            new Wagen() { Length = 15 } }
    });
    zugs.Add(new Zug()
    {
        Country = "Italia",
        Wagens = new List<Wagen>() {
            new Wagen() { Length = 28 },
            new Wagen() { Length = 28 }}
    });

    var result = zugs.GroupBy(
                train => train.Country.ToUpper(),
                train => train.Wagens,
                (train, carriages) =>
                new {
                    Country = train,
                    AvgCarriagesCount = carriages.Average(list => list.Count),
                    AvgCarriagesLength = carriages.SelectMany(carriage => carriage).Average(carr => carr.Length)
                });
}
```
![vs](https://github.com/jfuerlinger/maturavorbereitung-2020_21/blob/topic/LINQ-Kolev/LINQ/img/linq.png "groupBy results")  
#### __Elementoperatoren__  
Enthält die zurückgegebene Datenmenge keine Datensätze und wir versuchen mit First() auf den ersten Element zuzugreifen, kommt es zum „Kurzschluß“. Zum Beispiel wenn Name Max nicht vorhanden:  
```csharp
var teachers = Teachers
   .Where(t => t.Name == “Max”)
   .Select(t)
   .First();
```
Besser verwenden wir in diesem Fall *FirstOrDefault()*, der in dem Beispiel dann „NULL“ Wert zurückliefert. *First()* oder *FirstOrDefault()* sollen nicht verwendet werden, wenn mehrere Übereinstimmungen erwartet werden und wir möchten nur eine davon verarbeiten. Das wird natürlich funktionieren, aber *Single()* und *SingleOrDefault()*
sind bessere Auswahl. Sie lösen nämlich einer Ausnahme, wenn mehrere Übereinstimmungen vorliegen.
#### __Mengenoperatoren__  
LINQ definiert drei Operatoren, die einige allgemeine Mengenoperationen verwenden, um zwei Quellen zu kombinieren:
- **Intersect** erzeugt ein Ergebnis, das nur die Elemente enthält, die in beiden Eingangsquellen enthalten waren. 
- **Except** liefert nur die Elemente aus der ersten Eingabequelle, die nicht in der zweiten waren.
- Die Ausgabe von **Union** enthält Elemente, die in beide Eingangsquellen enthalten waren, jeweils einmal.
#### __Aggregatoperatoren__  
Diese Operatoren, zu denen *Count*, *Sum*, *Max*, **Min**, *Average* etc. gehören, setzen wir ein, wenn wir verschiedenste Berechnungen mit den Elementen der Datenquelle durchführen wollen.  
__Count__  
Es wird die Anzahl der Elemente in der abzufragenden Auflistung ermittelt.  
__Sum__  
Mit diesem Operator können Summen aus den Elementen der Quell-Auflistung gebildet werden.  

Der Operator **DefaultIfEmpty** gibt alle Elemente aus seiner Quelle zurück. Jedoch, wenn Quelle leer ist gibt für den einzelnes Element den (NULL, 0) Standardwert der Elementtyp zurück.  
### Verzögertes Ausführen von LINQ-Abfragen  
Normalerweise werden LINQ-Ausdrücke nicht bereits bei ihrer Definition, sondern erst bei Verwendung der Ergebnismenge ausgeführt.  
Das wird erreicht mit die Methoden:  
*ToArray()*, *ToList()*, *ToDictionary()*, *AsEnumerable()*, *Cast()* und *ToLookup()*. Die Methoden __ToArray()__ als auch __ToList()__ forcieren ein sofortiges Durchführen der Abfrage.  
### PLINQ (Parallel LINQ)
PLINQ ist eine parallele Implementierung von LINQ to Objects und kombiniert die Einfachheit und Lesbarkeit der LINQ Syntax mit der Leistungsfähigkeit der parallelen Programmierung. Es steht das komplette Angebot an Standard-Abfrageoperatoren zur Verfügung, zusätzlich gibt es spezielle Operatoren für parallele Operationen. Auf diese Weise können LINQ-Abfragen auf Multi-Core- / Multi-Prozessor-Systemen parallel ausgeführt werden, um sie (hoffentlich) zu beschleunigen.  
### Anhang Table 10-1  
|Operator|Zweck|
|--------|-------|
|Aggregate|Kombiniert alle Elemente über eine vom Benutzer bereitgestellte Funktion, um ein einzelnes Ergebnis zu erzielen|
|All|Gibt true zurück, wenn das angegebene Prädikat für keine Elemente false ist|
|Any|Gibt true zurück, wenn das angegebene Prädikat für mindestens ein Element true ist|
|Append|Gibt eine Sequenz mit allen Elementen aus der Eingabesequenz zurück, wobei am Ende ein Element hinzugefügt wird|
|AsEnumerable|Gibt die Sequenz als IEnumerable\<T\> zurück|
|AsParallel|Gibt eine ParallelQuery\<T\> für die parallele Abfrageausführung zurück|
|AsQueryable|Gewährleistet die Verwendung der IQueryable\<T\> -Handhabung, sofern verfügbar|
|Average|Berechnet das arithmetische Mittelwert der Elemente|
|Cast|Ändert jedes Element in der Sequenz auf den angegebenen Typ|
|Concat|Bildet eine Sequenz durch Verketten von zwei Sequenzen|
|Contains|Gibt true zurück, wenn sich das angegebene Element in der Sequenz befindet|
|Count, LongCount|Geben Sie die Anzahl der Elemente in der Sequenz zurück|
|DefaultIfEmpty|Erzeugt die Elemente der Quellsequenz, sofern keine vorhanden sind. In diesem Fall wird ein einzelnes Element mit dem Standardwert für den Elementtyp erzeugt|
|Distinct|Entfernt doppelte Werte|
|ElementAt|Gibt das Element an der angegebenen Position zurück|
|ElementAtOrDefault|Gibt das Element an der angegebenen Position zurück (erzeugt den Standardwert des Elementtyps, wenn er außerhalb des Bereichs liegt)|
|Except|Filtert Elemente aus der anderen bereitgestellten Sammlung heraus|
|First|Gibt das erste Element zurück und löst eine Ausnahme aus, wenn keine Elemente vorhanden sind|
|FirstOrDefault|Gibt das erste Element oder den Standardwert des Elementtyps zurück, wenn keine Elemente vorhanden sind|
|GroupBy|Sammelt Elemente in Gruppen|
|GroupJoin|Gruppiert Elemente in einer anderen Reihenfolge nach ihrer Beziehung zu Elementen in der Eingabesequenz|
|Intersect|Filtert Elemente heraus, die nicht in der anderen bereitgestellten Sammlung enthalten sind|
|Join|Erzeugt ein Element für jedes übereinstimmende Elementpaar aus den beiden Eingabesequenzen|
|Last|Gibt das letzte Element zurück und löst eine Ausnahme aus, wenn keine Elemente vorhanden sind|
|LastOrDefault|Gibt das letzte Element oder den Standardwert des Elementtyps zurück, wenn keine Elemente vorhanden sind|
|Max|Gibt den höchsten Wert zurück|
|Min|Gibt den niedrigsten Wert zurück|
|OfType|Filtert Elemente heraus, die nicht vom angegebenen Typ sind|
|OrderBy|Produziert Elemente in aufsteigender Reihenfolge|
|OrderByDescending|Produziert Elemente in absteigender Reihenfolge|
|Prepend|Gibt eine Sequenz zurück, die mit einem bestimmten Einzelelement beginnt, gefolgt von allen Elementen aus der Eingabesequenz|
|Reverse|Erzeugt Elemente in der entgegengesetzten Reihenfolge als die Eingabe|
|Select|Projiziert jedes Element über eine Funktion|
|SelectMany|Kombiniert mehrere Sammlungen zu einer|
|SequenceEqual|Gibt nur dann true zurück, wenn alle Elemente denen in der anderen angegebenen Reihenfolge entsprechen|
|Single|Gibt das einzige Element zurück und löst eine Ausnahme aus, wenn keine Elemente oder mehr als ein Element vorhanden sind|
|SingleOrDefault|Gibt das einzige Element oder den Standardwert des Elementtyps zurück, wenn keine Elemente vorhanden sind, löst eine Ausnahme aus, wenn mehr als ein Element vorhanden ist|
|Skip|Filtert die angegebene Anzahl von Elementen von Anfang an heraus|
|SkipWhile|Filtert Elemente von Anfang an heraus, solange sie einem Prädikat entsprechen|
|Sum|Gibt das Ergebnis der Addition aller Elemente zurück|
|Take|Erzeugt die angegebene Anzahl von Elementen und verwirft den Rest|
|TakeLast|Erzeugt die angegebene Anzahl von Elementen am Ende der Eingabe (alle Elemente davor werden verworfen)|
|TakeWhile|Erzeugt Elemente, solange sie mit einem Prädikat übereinstimmen, und verwirft den Rest der Sequenz, sobald eines nicht übereinstimmt|
|ToArray|Gibt ein Array zurück, das alle Elemente enthält|
|ToDictionary|Gibt ein Dictionary zurück, das alle Elemente enthält|
|ToHashSet|Gibt ein HashSet\<T\> zurück, das alle Elemente enthält|
|ToList|Gibt eine Liste\<T\> zurück, die alle Elemente enthält|
|ToLookup|Gibt eine mehrwertige assoziative Suche zurück, die alle Elemente enthält|
|Union|Erzeugt alle Elemente, die sich in einer oder beiden Eingaben befinden|
|Where|Filtert Elemente heraus, die nicht mit dem angegebenen Prädikat übereinstimmen|
|Zip|Kombiniert Elementpaare aus zwei Eingaben|
|||
#### Literaturverzeichnis

Mössenböck, Hanspeter: Kompaktkurs C# 6.0. 1. Auflage, Heidelberg: dpunkt.verlag GmbH, 2016  
Doberenz, Walter; Gewinnus, Thomas: Datenbankprogrammierung mit Visual C# 2010: Grundlagen, Rezepte, Anwendungsbeispiele, Köln: O’Reilly Verlag GmbH & Co.KG, 2010  
Griffiths, Ian; Programming C# 8.0. First Edition, Sebastopol: O’Reilly Media, Inc., 2019  
Lerman, Julia: Programming Entity Framework. 1st Edition, O’Reilly Media, Inc., 2009  

#### Internet Quellen
https://docs.microsoft.com/de-de/dotnet/api/system.linq?view=net-5.0  
http://dotnetpattern.com/linq-query-method-syntax  
