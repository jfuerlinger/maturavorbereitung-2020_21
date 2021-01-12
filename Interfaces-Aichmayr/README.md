# Interfaces


## Inhaltsverzeichnis:

- Definition
- Warum Interfaces?
- Merkmale von Interfaces
- Schnittstellendefinition
- Schnittstellenimplementierung
- Contract-First-Design
- Schnittstellen als Ersatz exakter Typangaben
- Ziele
- Einsatzgebiete
- Vertrag
- Technische Details
- Beispiele aus dem .Net-Framework
- Vor- & Nachteile
- IEnumerable & IEnumeration
- Referenzen


## Definition

Schnittstellen sind wie eine Vertragsvereinbarung. Sobald eine Klasse eine Schnittstelle implementiert, hat der auf ein Objekt dieser Klasse zugreifende Code die Garantie, dass die Klasse die Member der Schnittstelle aufweist. Mit anderen Worten: Eine Schnittstelle legt einen Vertragsrahmen fest, den die implementierende Klasse erfüllen muss.


## Warum Interfaces?

Bei mehrere Klassen ein gemeinsame Verhalten vorweisen sollen, kann man dies über die Vererbung erreichen.

### Das heißt:

- Klassen haben eine gemeinsame Basisklasse
- Alle abgeleiteten Klassen müssen Verhalten ausweisen
    - Bsp.: Eine abstrakte Methode `GetSalary()` wird in einer übergeordneten (abstrakten) Klasse `Employee` definiert.
            Alle abgeleiteten Klassen müssen `GetSalary()` implementieren.


- Andere Klassen, die nicht von `Employee` erben, aber auch `GetSalary()` implementieren, könnn nicht verwendet werden.
- Jede Klasse kann in `C#` nur eine Basisklasse haben (Mehrfachvererbung nicht möglich!)

Um Verhalten, Eigenschaft, Indexer und Ereignisse unviversell zu machen, muss ein Interface definiert werden. 


## Merkmale von Interfaces

- Die Namenskonvention bei Interfaces `C#` ist I + Klassenname IF: IPerson, Class: Person
    - So wird sichergestellt, dass sofort erkannt wird, dass es sich um ein Interface handelt.

- Eine Schnittstelle kann nicht direkt instanziert werden

- Eine Schnittstelle enhält keinen Implementierung von Methoden

- Eine Schnittstelle enthält keinen Konstruktor

- Eine Schnittstelle kann von einer oder mehreren Schnittstellen erben

- Member sind immer öffentlich und können keine Zugriffmodifizerer enthalten

- Schnittstellen können Ereignisse, Indexer, Methoden und Eigenschaften enthalten

- Klassen und Strukturen können mehrere Interfaces implementieren ("Mehrfachvererbung" in C#)

## Ziele

- Interfaces trennen den Entwurf von der Implementierung

- Interfaces legen Funktionalität fest, ohne auf die Implementierung einzugehen
  
- Beim Implementieren der Klasse ist die spatere Verwendung nicht von Bedeutung, sondern nur die bereitzustellende Funktionälitat
  
- Ein Anwender (eine andere Klasse) interessiert sich nicht für die Implementierungsdetails, sondern für die Funktionalität

## Schnittstellendefinition

Interfaces können:
- Indexer
- Ereignisse
- Methoden
- Eigenschaften
vorschreiben. Schnittstellen enthalten selbst keine Codeimplementierung, sondern nur abstrakte Definitionen.

```
public interface ICopy 
{
  string Caption {get; set;};
  void Copy();
}
```

## Schnittstellenimplementierung

Eine Schnittstelle ist wie ein Vertrag, den eine Klasse unterschreibt, sobald sie eine bestimmte Schnittstelle implementiert. 

### Das hat Konsequenzen: 
Eine Klasse, die eine Schnittstelle implementiert, muss ausnahmslos jedes Mitglied der Schnittstelle übernehmen. 
Eine zu implementierende Schnittstelle wird, getrennt durch einen Doppelpunkt, hinter dem Klassenbezeichner angegeben. In der Klasse werden alle `Member`, die aus der Schnittstelle stammen, mit den entsprechenden Anweisungen codiert.

```
class Document : ICopy {
  public void Copy() {
    Console.WriteLine("Das Dokument wird kopiert.");
  }
  public string Caption {
    get{ [...] }
    set{ [...] }
  }
  [...] 
}
```

Grundsätzlich kann man jeden beliebigen Code in die Schnittstellenmethoden schreiben. Das ist aber nicht Sinn und Zweck. Stattdessen sollten Sie sich streng daran halten, was die Dokumentation beschreibt. Das bedeutet im Umkehrschluss aber auch, 
dass eine Schnittstelle ohne Dokumentation wertlos ist. Nur die Dokumentation gibt Auskunft darüber, was eine Methode leisten soll und wie ihre Rückgabewerte zu interpretieren sind.

Eine Klasse ist nicht nur auf die Implementierung einer Schnittstelle beschränkt, es dürfen – im Gegensatz zur Vererbung – auch mehrere sein, die durch ein Komma voneinander getrennt werden.

```
class Document : ICopy, IDisposable {
  [...]
}
```

## Contract-First-Design

Beim `Contract-First-Design` werden zuerst die Funkionalitäten des Interfaces definiert und danach erst die Funktionalitäten in der Klasse implementiert.


- Konzept, um qualitativ hochwertige Implementierung zu erstellen
- Erlaubt Verwendung unterschiedlicher Implementierungen
- Klassenname und Vererbungshierarchie der konkreten Implementierung ist egal


Dadurch wird sichergestellt, dass alle Klassen die selben Funktionalitäten aufweisen, den selben Vertrag "unterschreiben".
--> Polymorphie


## Schnittstellen als Ersatz exakter Typangaben

### Bsp.:

```
   public DoSomething(IAny parameter){
       parameter.Action();
   }
```

Der Parameter ist vom Typ der Schnittstelle `IAny`. 


Der Parameter verlangt, dass das ihm übergebene Argument ein Objekt ist, das die Schnittstelle `IAny` implementiert – egal, ob das Objekt vom Typ DemoClass, Circle, Auto oder Person ist.

Wie schon erwehnt kann eine Schnittstelle wie eine Basisklasse betrachtet werden. Dadurch, dass ein Parameter vom Typ einer Schnittstelle definiert ist, wird uns vorgeschrieben, dass die Member der Schnittstelle von der Klasse implementiert sind. Im Fall von IComparer handelt es sich um die Methode Compare, die zwei Objekte des angegebenen Arrays miteinander vergleicht. Welche weiteren Member sich noch in der Klassendefinition befinden, interessiert in diesem Zusammenhang nicht.

## Einsatzgebiete








## Technische Details





## Beispiele aus dem .Net-Framework

```
using System;
namespace Übung 1
{
  public class EinfacheSchnittstellen
  {
    static void Main(string[] args)
    {
      Mathe m = new Mathe();
      DruckeVersionsInfo(m);
      Console.ReadLine();
    }
    private static void DruckeVersionsInfo(IVersionsInfo vi)
    {
      Console.WriteLine(vi.GetType().ToString());
      Console.WriteLine(vi.GetAutor());
      Console.WriteLine(vi.GetVersion());
    }
  }


  public interface IVersionsInfo
  {
    string GetAutor();
    string GetVersion();
  }


  public class Mathe: IVersionsInfo
  {
    private static string version = "0.0.97.123";
    private static string autor = "Dirk Frischalowski";
    public string GetAutor()
    {
      return autor;
    }
    public string GetVersion()
    {
      return version;
    }
  }
}
```


## Vor- & Nachteile


## Referenzen
