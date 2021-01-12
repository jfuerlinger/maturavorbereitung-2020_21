# Interfaces

## Inhaltsverzeichnis:

- Definition
- Merkmale von Interfaces
- Schnittstellendefinition
- Schnittstellenimplementierung
- Contract-First-Design
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


## Merkmale von Interfaces

- Die Namenskonvention bei Interfaces (C#) ist I + Klassenname IF: IPerson, Class: Person
    - So wird sichergestellt, dass sofort erkannt wird, dass es sich um ein Interface handelt.

- Eine Schnittstelle kann nicht direkt instanziert werden

- Eine Schnittstelle enhält keinen Implementierung von Methoden

- Eine Schnittstelle enthält keinen Konstruktor

- Eine Schnittstelle kann von einer oder mehreren Schnittstellen erben

- Member sind immer öffentlich und können keine Zugriffmodifizerer enthalten

- Schnittstellen können Ereignisse (Event), Indexer, Methoden und Eigenschaften (Property) enthalten

- Klassen und Strukturen können mehrere Interfaces implementieren ("Mehrfachvererbung" in C#)

## Schnittstellendefinition

Interfaces können:
- Methoden
- Eigenschaften (Properties)
vorschreiben. Schnittstellen enthalten selbst keine Codeimplementierung, sondern nur abstrakte Definitionen. Schauen wir uns dazu eine einfache, fiktive Schnittstelle an:


```
public interface ICopy 
{
  string Caption {get; set;};
  void Copy();
}
```

## Schnittstellenimplementierung

Bei der Vererbung wird von Ableitung gesprochen, analog hat sich bei den Schnittstellen der Begriff Implementierung geprägt. Eine Schnittstelle ist wie ein Vertrag, den eine Klasse unterschreibt, sobald sie eine bestimmte Schnittstelle implementiert. 

### Das hat Konsequenzen: 
Eine Klasse, die eine Schnittstelle implementiert, muss ausnahmslos jedes Mitglied der Schnittstelle übernehmen. 
Eine zu implementierende Schnittstelle wird, getrennt durch einen Doppelpunkt, hinter dem Klassenbezeichner angegeben. In der Klasse werden alle Member, die aus der Schnittstelle stammen, mit den entsprechenden Anweisungen codiert.

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

Grundsätzlich können Sie jeden beliebigen Code in die Schnittstellenmethoden schreiben. Das ist aber nicht Sinn und Zweck. Stattdessen sollten Sie sich streng daran halten, was die Dokumentation beschreibt. Das bedeutet im Umkehrschluss aber auch, 
dass eine Schnittstelle ohne Dokumentation wertlos ist. Nur die Dokumentation gibt Auskunft darüber, was eine Methode leisten soll und wie ihre Rückgabewerte zu interpretieren sind.

Eine Klasse ist nicht nur auf die Implementierung einer Schnittstelle beschränkt, es dürfen – im Gegensatz zur Vererbung – auch mehrere sein, die durch ein Komma voneinander getrennt werden.

```
class Document : ICopy, IDisposable {
  [...]
}
```

## Contract-First-Design

- Konzept, um qualitativ hochwertige Implementierung zu erstellen
- Erlaubt Verwendung unterschiedlicher Implementierungen
- Klassenname und Vererbungshierarchie der konkreten Implementierung ist egal



## Ziele

- Interfaces trennen den Entwurf von der Implementierung

- Interfaces legen Funktionalität fest, ohne auf die Implementierung einzugehen
  
- Beim Implementieren der Klasse ist die spatere Verwendung nicht von Bedeutung, sondern nur die
  bereitzustellende Funktionälitat
  
- Ein Anwender (eine andere Klasse) interessiert sich nicht
  für die Implementierungsdetails, sondern für die Funktionalität

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
