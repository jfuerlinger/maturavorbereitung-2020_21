# Interfaces


## Inhaltsverzeichnis:

- Definition
- Warum Interfaces?
- Merkmale von Interfaces
- Vertrag
- Schnittstellendefinition
- Schnittstellenimplementierung
- Contract-First-Design
- Schnittstellen als Ersatz exakter Typangaben
- Ziele
- Einsatzgebiete
- Technische Details
- Beispiele aus dem .Net-Framework
- Vor- & Nachteile
- IEnumerable & IEnumeration
- Referenzen

## Definition

Schnittstellen sind wie eine Vertragsvereinbarung. Sobald eine Klasse eine Schnittstelle implementiert, hat der auf ein Objekt dieser Klasse zugreifende Code die Garantie, dass die Klasse die Member der Schnittstelle aufweist. Mit anderen Worten: Eine Schnittstelle legt einen Vertragsrahmen fest, den die implementierende Klasse erfüllen muss.

## Warum Interfaces?

Wenn mehrere Klassen ein gemeinsames Verhalten vorweisen sollen, kann man dies über die Vererbung erreichen.

### Das heißt:

- Klassen haben eine gemeinsame Basisklasse
- Alle abgeleiteten Klassen müssen Verhalten ausweisen
    - Bsp.: Eine abstrakte Methode `GetSalary()` wird in einer übergeordneten (abstrakten) Klasse `Employee` definiert.
            Alle abgeleiteten Klassen müssen `GetSalary()` implementieren.


- Andere Klassen, die nicht von `Employee` erben, aber auch `GetSalary()` implementieren, könnn nicht verwendet werden.
- Jede Klasse kann in `C#` nur eine Basisklasse haben (Mehrfachvererbung nicht möglich!)

Um Verhalten, Eigenschaft, Indexer und Ereignisse unviversell zu machen, muss ein Interface definiert werden. 

Schnittstellen unterstützen das Konzept des Information Hiding und sollten möglichst stabil sein. Schnittstellen erlauben es mehreren Teams unabhängig von einander zu arbeiten. Sie erlauben eine Trennung von

- ### Spezifikation und
- ### Implementierung


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

## Vertrag

- Vertrag wird in `C#` über Interface beschrieben

- Vertrag erweitern statt Vertrag ändern --> Führt zu neuem Vertrag

### Das heißt:

- Änderungen im Vertrag
    - Alle "Nutzer" des Vertrages müssen geändert werden

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

## Einsatzgebiete

## Technische Details

### Was sind die Unterschiede zwischen abstract und interface?

| Tables          | Abstract Class                                                           | Interface                                                                 |
| --------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Prüfung         | Prüfung zur Komplierzeit                                                 | Prüfung zur Komplierzeit                                                  |
| Implementierung | Können implementierungen enthalten                                       | Enthalten keine implementierungen (lediglich deklarationen)               |
| Vererbung       | Eine Klasse kann nur von einer Abstrakten Klasse erben                   | Eine Klasse kann jedoch eine beliebige Anzahl an Interface implementieren |
| Access Modifier | Abstrakte Klassenmember können Zugriffsmodifier enthalten                | Alle Interface Member sind automatisch public                             |
| Erlaubte Member | Fields, Properties, Construcotrs, Destructors, Methods, Events, Indexers | Properties, Methods, Events, Indexers                                     |


## Beispiele aus dem .Net-Framework

### Beispiel 1:
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


### Beispiel 2:
```
using System;
namespace Übung 2
{
  public class Liste mit Interface
  {
    static void Main(string[] args)
    {
            List<IAnimal> animals = new List<IAnimal>();
            animals.Add(new Dog("Fido"));
            animals.Add(new Cat("Bob"));
            animals.Sort();

            foreach(var animal in animals)
                Console.WriteLine(animal.Describe());
            Console.ReadKey();
    }
  }

  public interface IAnimal{
        string Describe();

        string Name
        {
            get;
            set;
        }
  }

  public class Dog : IAnimal, IComparable
  {
    public Dog(string name){
      Name = name;
    }

    public string Describe()
    {
      return "Hello, I'm a dog and my name is " + this.Name;
    }

    public int CompareTo(object obj)
    {
      if(obj is IAnimal)
      {
        return this.Name.CompareTo((obj as IAnimal).Name);
      }
      else
      {
        return 0;
      }
    }
  }

    public class Cat : IAnimal, IComparable
  {
    public Cat(string name){
      Name = name;
    }

    public string Describe()
    {
      return "Hello, I'm a cat and my name is " + this.Name;
    }

     public int CompareTo(object obj)
    {
      if(obj is IAnimal)
      {
        return this.Name.CompareTo((obj as IAnimal).Name);
      }
      else
      {
        return 0;
      }
    }
  }
}

```

## ICompareable & IComparer
- IComparable 
  - Die Rolle von IComparable besteht darin, eine Methode zum Vergleichen von zwei Objekten eines bestimmten Typs bereitzustellen.    Es ist erforderlich, wenn eine beliebige Sortierfunktion für das Objekt vorgesehen ist.

```
int IComparable.CompareTo(object obj)
{
   car c=(car)obj;
   return String.Compare(this.make,c.make);
}
```
- IComparer 
  - Die Rolle von IComparer besteht darin, zusätzliche Vergleichs Mechanismen bereitzustellen. Beispielsweise kann die                Reihenfolge der Klasse für mehrere Felder oder Eigenschaften, aufsteigende und absteigende Reihenfolge auf demselben Feld oder    beides bereitstellen.

```
private class sortYearAscendingHelper : IComparer
{
   int IComparer.Compare(object a, object b)
   {
      car c1=(car)a;
      car c2=(car)b;
      if (c1.year > c2.year)
         return 1;
      if (c1.year < c2.year)
         return -1;
      else
         return 0;
   }
}
```

## IEnumerable & IEnumerator

Enumerable und IEnumerator sind zwei Schnittstellen, die zum Implementieren der Iteration in .NET verwendet werden. In `C#` implementieren alle Collections  wie Listen, Dictionaries usw. die IEnumerable-Schnittstelle. Damit sie iteriert werden können.

Jede Klasse, die die IEnumerable-Schnittstelle implementiert, kann aufgelistet werden. Das heißt, man kann eine `foreach-Schleife `verwenden, um die Klasse zu durchlaufen.

- ### IEnumerable
Um die Schnittstelle `IEnumerable` verwenden zu können, wird die Schnittstelle `IEnumerator` & (`IDispose`) benötigt.

IEnumerable ist eine Schnittstelle, die eine einzelne Methode `GetEnumerator()` definiert, die eine IEnumerator-Schnittstelle     zurückgibt.
Dies funktioniert für den schreibgeschützten Zugriff auf eine Collection, die implementiert, dass IEnumerable mit einer           `foreach-Schleife ` verwendet werden kann.

```
class Items : IEnumerable
{
    private string[] ItemList = new string[10];
    int pointer = 0;

    public void AddItem(string item)
    {
        ItemList[pointer++] = item;
    }

    public void RemoveItem(int index)
    {
        ItemList[index] = "";
    }

    public IEnumerator GetEnumerator()
    {
        return new ItemEnumerator();
    }
}
```

- ### IEnumerator 

`IEnumerator` verfügt über zwei Methoden: 
- MoveNext
  - Gibt `true` zurück, um anzuzeigen, dass das Ende der Collection nicht erreicht haben und gibt `false` zurück,  wenn wir den   letzten Index der Collection erreicht haben.
- Reset
  - Setzt den Zeiger der Collection zurück.

Ein Property:
- Current
  - GIbt das aktuelle Element zurückzugeben.


```
class ItemEnumerator : IEnumerator
{  
    int index=0;
    string[] ItemList;

    public ItemEnumerator(ref string[] List)
    {
        ItemList = List;
    }

    public object Current => ItemList[index++];  

    public bool MoveNext()
    {
        return index >= ItemList.Length ? false: true;
    }

    public void Reset()
    {
        index = 0;
    }
}
```
- ### Verwendung
```
class Program
{
    static void Main(string[] args)
    {
        Items i = new Items();
        i.AddItem("Item 1");
        i.AddItem("Item 2");
        i.AddItem("Item 3");

        foreach (var items in i)
        {
            Console.WriteLine(items);
        }
        Console.ReadKey();
    }
}
```

- ### Merke:
Eine Collection wie z.B. (List, Dictionary, Array usw.) können die `foreach-Schleife` verwenden, da sie selbst den Vertrag `IEnumerable` unterzeichnet haben.


## Vor- & Nachteile

## Referenzen
