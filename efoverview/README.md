# EF CodeFirst - Überblick	

## Inhaltsverzeichnis:

- Was ist Entity Framework?
- Architektur
- Varianten
- CodeFirst Grundlagen (Convention over Configuration, Annotationen, Relationen, Vererbung, Concurrency, ...)


## Was ist Entity Framework?
Entity Framework, kurz auch EF, ist ein Framework für objektrelationale Abbildung(ORM). Es wurde von Microsoft entwickelt und dient dem ORM auf .NET-Objektstrukturen. Die erste finale Version erschien als Teil des .NET Framework 3.5 (Service Pack 1) im Jahr 2008. Ab der Version 6.0, die 2013 erschien, gehört das Framework nicht mehr zum .NET Framework.
Einhergehend mit .NET Core gibt es seit 2016 das separate Framework Entity Framework Core, das auch EF Core genannt wird. Seit der .NET Core Version 3 ist dieses ein Zusatzpaket und nicht mehr automatisch Bestandteil von .NET Core.

#### Was ist ORM?
Objektrelationale Abbildung (englisch object-relational mapping, ORM) ist eine Technik der Softwareentwicklung, mit der ein in einer objektorientierten Programmiersprache geschriebenes Anwendungsprogramm seine Objekte in einer relationalen Datenbank ablegen kann.

#### Entity Framework Features
- **Cross-platform**
- **Modelling** 
EF (Entity Framework) erstellt ein EDM (Entity Data Model) auf Basis von POCO (Plain Old C# Object) Entitäten. Die Model Klasse wird bei Querying oder beim Speichern von Entity Daten auf der Datenbank benützt. 
- **Querying**
Durch EF ist es möglich Daten von der Datenbank in Form von Linq Abfragen aufzurufen. Der Datenbank Provider übersetzt diese LINQ-Abfragen in die datenbankspezifische Abfragesprache (z.B. SQL für eine relationale Datenbank).  
- **Change Tracking** 
Veränderungen bei Entitäten (Property Values) werden automatisch erkannt.
- **Saving** 
EF führt Insert, Update, Delete Befehle auf der Datenbank aus bei Aufruf der Methode `SaveChanges()`. EF bietet auch die asynchrone Methode `SaveChangesAsync()`.
- **Migrations** 
EF bietet eine Reihe von Migration Commands, die über die NuGet Package Manager Konsole ausgeführt werden können. Im Entwicklungsprozess wird bei jeder Änderung am Model die DB auf den neuen Stand migriert.

## Architektur
![alt text](https://www.entityframeworktutorial.net/Images/ef-architecture.PNG)

- EDM (Entity Data Model): Das EDM besteht aus drei Hauptteilen - Conceptual Model, Mapping und Storage Model.

- Conceptual Model: Das konzeptionelle Modell enthält die Modellklassen und ihre Beziehungen. 
Storage Model: Das Storage Model ist das Datenbankdesignmodell, sie enthält die Tabellen, Views, Stored Procedures, Beziehungen und Schlüsseln. 

- Mapping: Das Mapping holt sich die Informationen durch die Abbildung vom Conceptual Model zum Storage Model.

- LINQ to Entities: LINQ-to-Entities (L2E) ist eine Abfragesprache für Instanzen in der Model. Die Abfragen geben die Entitäten, welche in der Conceptual Model definiert sind, zurück. 

- Entity SQL: Entity SQL ist eine weitere Abfragesprache (nur für EF 6). 

- Object Service: Object service bietet Zugriff auf die Daten von der Datenbank. Sie ist veranwortlich für die Materialization (Konvertierung der zurückgegebenen Daten von einem Entity Client Data Provider in einem Entity Object Structure).

- Entity Client Data Provider: Der Entity Client Data Provider ist hauptsächlich für die Konvertierung von LINQ-To-Entities und Entity SQL Abfragen in geeigente SQL Abfragen für die hintergelegte Datenbank zuständig. Zusätzlich kommuniziert er mit dem ADO.Net Data Provider, der wiederum Daten aus der Datenbank sendet und abruft. 

- ADO.Net Data Provider: Diese Schicht kommuniziert mit der Datenbank über das Standard ADO.Net.

## Varianten

| 		 						   | Code Centric | Designer Centric |
|--------|--------		|--------		   |
|    Keine Datenbank vorhanden    |       (**Code First**) Bestehende Klassen werden mit Annotationen `(Table, Column)` ausgezeichnet, welche die Abbildung auf eine Datenbank steuern. Darauf aufbauend werden vom DbContext die Datenbank und die Datenbank-Tabellen modelliert und beim Aufruf der `SaveChanges()`-Methode erstellt.       |		   (**Model First**) Die Entity-Klassen werden mit einem grafischen Designer modelliert. Das Modell wird einerseits mit Hilfe des Text Template Transformation Toolkit (T4) und der zugehörigen T4-Skriptsprache in Entity-Klassen umgewandelt. Zudem erlaubt es der Designer, ein SQL-Skript zu erstellen, mit dem die Datenbank erstellt wird.                                                                       		   |
|Verwendung einer bestehenden Datenbank|(**Code First**) Die Entity-Klassen können entsprechend der vorgegebenen Datenbank manuell erstellt, modelliert und ausgezeichnet werden. Dies ist jedoch sehr arbeitsintensiv.|(**Database First**) Mit Hilfe eines Assistenten wird die Datenbank abgefragt und entsprechend der Datenbankstruktur ein passendes Modell erstellt. Dieses wird mit einem T4-Skript in die entsprechenden Klassen umgewandelt.|

## CodeFirst Grundlagen

## Context Class 

Die Context Class ist die wichtigste Klasse in Bezug EF 6 bzw. EF Core. Sie repräsentiert eine Session mit der hintergelegten Datenbank unter Verwendung von CRUD (Create, Read, Update, and Delete) Operationen.
Die Context Klasse leitet sich von der System.Data.Entity.DbContext ab. 
Eine Instanz der Context Klasse repräsentiert das Unit Of Work und das Repository Pattern, in der mehrere Veränderungen unter einer einzelnen Datenbank Transaktion vereint werden.

Zusätzlich wird die Klasse für Abfragen, um Daten in der Datenbank zu speichern, benützt.



Beispiel für eine Context Klasse:
``` C#
public class SchoolContext : DbContext  
{  
    public SchoolContext()  
    {  


    }  

    // Entities         
    public DbSet<Student> Students { get; set; }  
    public DbSet<StudentAddress> StudentAddresses { get; set; }  
    public DbSet<Grade> Grades { get; set; }  
}

```

- - -
## Convention over Configuration
Viele Eigenschaften werden per Konvention festgelegt, diese können aber angepasst werden, entweder durch Annotationen oder eine eigene API (Fluent-API). Die Fluent-API ist mächtiger als Annotationen und erlaubt „saubere“ Modelklassen (Entitäten).

#### Primary Key
Wenn ein Property im Namen `ID` beinhaltet, wird sie automatisch als Primary Key konfiguriert. EF Core bevorzugt ausschließlich `ID`, für den Fall falls die Klasse beides enthält.

#### Foreign Key
Propertyname bezeichnet die „Gegenseite“ und endet auf Id.
CodeFirst verwendet dieses Property als FK, sonst muss die Annotation ForeignKey verwendet werden.

``` C#
public class Book
{
    public int Id { get; set; }
    public string Title { get; set;}
}

public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
}

```
#### Tables
Das Entity wird einer Table zugeordnet mit demselben Namen so wie die zugehörende Property `DbSet<TEntity>`.
``` C#
public class LibraryContext : DbContext
{
    public DbSet<Book> Books { get; set; }
}

public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public int PublisherId { get; set; }
    public Publisher Publisher { get; set; }
}

public class Publisher
{
    public int PublisherId { get; set; }
    public string Name { get; set; }
    public ICollection<Book> Books { get; set; }
}
```

Das `DbSet<TEntity>` repräsentiert eine Sammlung von der Entität, sie ermöglicht auch die Datenoperationen (CRUD).
Veränderungen werden nur übernommen, wenn die Methode `SaveChanges()` aufgerufen wird.

Beispiel: Entität hinzufügen 
``` C#
var author = new Author{
    FirstName = "William",
    LastName = "Shakespeare"
};

using (var context = new SampleContext())
{
    context.Authors.Add(author); // Author wird im Speicher hinzufügt
    context.SaveChanges(); // commit, Veränderungen werden auf der Datenbank übernommen
}
```

## Annotationen
Beispiel:
``` C#
public class Pupil
{
    public int Id { get; set; }
    [MaxLength(50)]
    [Required]
    public string LastName { get; set; }
    [MaxLength(50)]
    public string ClassName { get; set; }
    public City City { get; set; }
    public DateTime? BirthDate { get; set; }
    public string Address { get; set; }
}

```
#### Gängige Annotiationen
[Required] – Field ist required und not nullable. Kann auch für Beziehungen verwendet werden

[NotMapped] – Property wird nicht in DB gemappt
Z.B. Berechnete Properties

[MaxLength(…)], [MinLength(…)]

[Table("Table name")] – Name der DB-Tabelle für die Entität

[Key] – Name des Primärschlüssels

[ForeignKey("EntityId")] - Fremdschlüssel

[Column(TypeName = "image")] – Byte-Array soll als Image abgespeichert werden

## Relationen
Eine Beziehung definiert, wie zwei Entitäten miteinander in Beziehung stehen. In einer relationalen Datenbank wird dies durch eine FOREIGN KEY-Einschränkung repräsentiert.

#### Navigation Properties
Das gängigste Muster für Beziehungen besteht darin, dass für beide Enden der Beziehung Navigation Properties und eine Fremdschlüssel Property definiert sind, die in der abhängigen Entitätsklasse definiert ist.
Wenn zwischen zwei Typen ein paar Navigation Properties gefunden werden, werden diese als Inverse Navigation Properties derselben Beziehung konfiguriert.

``` C#
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

Eine einzelne Navigation Property reicht aus, um eine Beziehung gemäß der Konvention zu definieren.

``` C#
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
}

```

#### m:n
``` C#

public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public Author Author { get; set; }
    public ICollection<Category> Categories { get; set; }
} 
 
public class Category
{
    public int CategoryId { get; set; }
    public string CategoryName { get; set; }
    public ICollection<Book> Books { get; set; }
}

```

#### 1:1
``` C#
public class Author
{
    public int AuthorId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public AuthorBiography Biography { get; set; }
}

public class AuthorBiography
{
    public int AuthorBiographyId { get; set; }
    public string Biography { get; set; }
    public DateTime DateOfBirth { get; set; }
    public string PlaceOfBirth { get; set; }
    public string Nationality { get; set; }
    public int AuthorId { get; set; }
    public Author Author { get; set; }
}
```

## Vererbung
Gemäß der Konvention scannt EF nicht automatisch nach Basis-oder abgeleiteten Typen.
Standardmäßig ordnet EF die Vererbung mithilfe des TPH-Pattern (Table per Hierarchy) zu. TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie, und eine Diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt. Entity Framework Core implementiert nur das TPH-Pattern.

``` C#
public class Contract
{
    public int ContractId { get; set; }
    public DateTime StartDate { get; set; }
    public int Months { get; set;}
    public decimal Charge { get; set; }
}

public class MobileContract : Contract
{
    public string MobileNumber { get; set; }
}

public class TvContract : Contract
{
    public PackageType PackageType { get; set; }
}

public class BroadBandContract : Contract
{
    public int DownloadSpeed { get; set; }
}

public enum PackageType
{
    S, M, L, XL
}

```

## Concurrency

Concurrency ist ein Verfahren, um beispielsweise in Warenwirtschaftssystemen den parallelen Zugriff von mehreren Benutzern auf denselben Datensatz konfliktarm und ohne Inkonsistenzen zu regeln.

#### Pessimistic Concurrency 
Pessimistic Concurrency (Pessimistisches Locking) beim Zugriff eines Benutzers auf den Datensatz ist der Schreib- und Lesezugriff für alle anderen Nutzer gesperrt. Es ist damit also für weitere Benutzer nicht mehr möglich, einen Datensatz aufzurufen oder auszudrucken, bis der Datensatz wieder freigegeben wird. Entity Framework Core hat keinen Support dafür.

#### Optimistic Concurrency
Optimistic Concurrency wird mehreren Nutzern Parallelzugriff gewährt. Somit haben alle Benutzer grundsätzlich Leserechte, um z. B. Artikelinformationen aufzurufen oder auszudrucken. Wenn aber ein Artikel von einem der Benutzer geändert wurde, so bekommen andere Nutzer, die denselben Datensatz fast gleichzeitig zu ändern versuchen, eine Benachrichtigung, dass der Artikel aktualisiert wurde. In diesem Fall ist also der Nutzer privilegiert, der zuerst den Datensatz geändert hat. Wenn der Nutzer den Datensatz verlässt, wird ihm sein Privileg wieder entzogen, und ein nächster Anwender erhält Schreibzugriff. Entity Framework bietet Unterstützung für das Optimistic Concurrency Management. 

#### Behandlung von Concurrency Konflikten
```C#
public class Person
{
    public int PersonId { get; set; }
    [ConcurrencyCheck]
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

```

##### Timestamp/rowversion
Ein Timestamp/rowversion-Objekt ist ein Property, für die ein neuer Wert automatisch von der Datenbank generiert wird, wenn eine Zeile eingefügt oder aktualisiert wird. Das Property wird auch als Concurrency Token behandelt. Die genauen Details hängen vom verwendeten Datenbankanbieter ab, für SQL Server wird normalerweise eine Byte [] -Eigenschaft verwendet, die als rowversion Spalte in der Datenbank eingerichtet wird.

```C#
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    [Timestamp]
    public byte[] Timestamp { get; set; }
}
```
## Migration
Code First Migrations ist eine Reihe von Powershell-Skripten, welche die Datenbankmigration erleichtern.

#### Migration erstellen
``` C#
[Command Line]
dotnet ef migrations add <name of migration>
[Package Manager console]
add-migration <name of migration>
```

#### Migration Updaten
``` C#
[Command line]
dotnet ef database update Create
[Package Manager Console]
update-database Create
```

#### Migration löschen 
``` C#
[Command Line]
dotnet ef migrations remove
[Package Manager Console]
remove-migration
```

