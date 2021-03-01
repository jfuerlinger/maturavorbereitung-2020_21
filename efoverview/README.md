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
EF (Entity Framework) erstellt ein EDM (Entity Model) auf Basis von POCO (Plain Old C# Object) Entitäten. Die Model Klasse wird bei Querying oder beim Speichern von Entity Daten auf der Datenbank benützt. 

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

EDM (Entity Data Model): Das EDM besteht aus drei Hauptteilen - Conceptual Model, Mapping und Storage Model.

Conceptual Model: Das konzeptionelle Modell enthält die Modellklassen und ihre Beziehungen. 

Storage Model: Das Storage Model ist das Datenbankdesignmodell, sie enthält die Tabellen, Views, Stored Procedures und deren Relationen/Keys. 
 
Mapping: Das Mapping holt sich die Informationen durch die Abbildung vom Conceptual Model zum Storage Model.

LINQ to Entities: LINQ-to-Entities (L2E) ist eine Abfragesprache für Instanzen in der Model. Die Abfragen geben die Entitäten, welche in der Conceptual Model definiert sind, zurück. 

Entity SQL: Entity SQL ist eine weitere Abfragesprache (nur für EF 6). 

Object Service: Object service ist der Main Entry Point für die Daten auf der Datenbank. Sie ist veranwortlich für die Materialization (Konvertierung der Daten von einem Entity Client Data Provider in einem Entity Object Structure).

Entity Client Data Provider: 
Der Entity Client Data Provider ist hauptsächlich für die Konvertierung von LINQ-To-Entities und Entity SQL Abfragen in geeigente SQL Abfragen für die hintergelegte Datenbank zuständig. Zusätzlich kommuniziert er mit dem ADO.Net Data Provider, der wiederum Daten aus der Datenbank sendet und abruft. 

ADO.Net Data Provider: 
Diese Schicht kommuniziert mit der Datenbank über das Standard ADO.Net.

## Varianten


|                 | Code Centric                                                           | Designer Centric                                                                |
| --------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Keine Datenbank vorhanden         | (**Code First**) Bestehende Klassen werden mit Annotationen `(Table, Column)` ausgezeichnet, welche die Abbildung auf eine Datenbank steuern. Darauf aufbauend werden vom DbContext die Datenbank und die Datenbank-Tabellen modelliert und beim Aufruf der `SaveChanges()`-Methode erstellt.                                         | (**Model First**) Die Entity-Klassen werden mit einem grafischen Designer modelliert. Das Modell wird einerseits mit Hilfe des Text Template Transformation Toolkit (T4) und der zugehörigen T4-Skriptsprache in Entity-Klassen umgewandelt. Zudem erlaubt es der Designer, ein SQL-Skript zu erstellen, mit dem die Datenbank erstellt wird.                                          |
| Verwendung einer bestehenden Datenbank | (**Code First**) Die Entity-Klassen können entsprechend der vorgegebenen Datenbank manuell erstellt, modelliert und ausgezeichnet werden. Dies ist jedoch sehr arbeitsintensiv.                                       | (**Database First**) Mit Hilfe eines Assistenten wird die Datenbank abgefragt und entsprechend der Datenbankstruktur ein passendes Modell erstellt. Dieses wird mit einem T4-Skript in die entsprechenden Klassen umgewandelt.              |





## CodeFirst Grundlagen

#### Convention over Configuration

Viele Eigenschaften werden per Konvention festgelegt, diese können aber angepasst werden.
Anpassung durch Annotationen oder eigene API (Fluent-API)
Fluent-API ist mächtiger als Annotationen
Fluent-API erlaubt „saubere“ Modelklassen (Entitäten)

#### Annotationen

#### Relationen

#### Vererbung

#### Concurrency

#### Migration
