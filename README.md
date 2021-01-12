# Maturavorbereitung 2020/21

## Allgemeines

Als Vorbereitung zur Matura werden die wichtigsten Themen aus dem Unterricht nochmals von den Schülern zusammengefasst. Diese Zusammenfassung dient dann als Fragenkatalog zur Programmier-Matura.

## Ablauf

1. Jeder Schüler sucht sich ein Thema aus (**Frist: 22.12.2021, 21:25 Uhr**), welches selbstständig bearbeitet werden muss! Info per MS Teams an Josef Fürlinger (inkl. Name des GitHub-Accounts des Schülers).

2. Zur Ausarbeitung wird durch den Schüler ein eigener Branch mit der Namenskonvention "topic/thema-name" erstellt.

3. Die Ausarbeitung erfolgt per Markdown in einem eigenen Sub-Folder (siehe dazu die Vorlage -> `./collections-und-generics/README.md`) im jeweiligen `topic`-Branch des Schülers. Zusätzlich bitte die eigene README.md-Datei (Subfolder) in der Themenliste unten verlinken!
   Hinweise zur Verwendung von Markdown (Tabellen, Code-Fragmente, Bilder, etc.) finden Sie hier: [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

4. Wenn ein Teil der Ausarbeitung abgeschlossen ist - **jedoch spätestens zwei Wochen vor der Präsentation** - wird per Pullrequest durch den Schüler die Ausarbeitung in den `main`-Branch übernommen. Diese Änderungen werden durch die Lehrperson freigegeben oder gegebenfalls kommentiert umd mit weiteren Anweisungen versehen. Siehe dazu folgendes Tutorial: [GitHub Tutorial (Branches, Pullrequests, etc.)](https://guides.github.com/activities/hello-world).

5. Die Ausarbeitung wird vor der Klasse präsentiert (**mind. 15 Minuten und max. 30 Minuten**) und anschließend in einer Diskussion verteidigt.

6. Pro Woche werden ca. drei Themen präsentiert.

## Themen

| Thema                                                          | Beschreibung                                                                                                                                        |       Schüler        |   Datum    |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------:|:----------:|
| [Collections und Generics](collections-und-generics/README.md) | Generische Methoden und Klassen, Constraints in C#, Arten von Collections, Einsatz von Generics in Collections                                      |  Stefan Steininger   | 19.01.2021 |
| [Exceptionhandling](exceptionhandling/README.md)               | Ziele, Grundkonzepte des Exceptionhandlings, Definition eigener Exceptions, Verwendung von geschachtelten Exceptions                                |         TBD          | 19.01.2021 |
| Muster "Singleton"                                             | Aufbau und Einsatzgebiete, Unterschied zu statischer Klasse                                                                                         |         TBD          | 19.01.2021 |
| Muster "Observer"                                              | Aufbau und Einsatzgebiete, Beteiligte Akteure, Implementierungsvarianten (Interfaces, Delegates, Events)                                            |         TBD          | 26.01.2021 |
| Muster "Iterator"                                              | Aufbau und Einsatzgebiete, Beteiligte Akteure, Best Practices                                                                                       |         TBD          | 26.01.2021 |
| Referenz- und Wertetypen                                       | Grundlegende Unterschiede, Boxing/Unboxing in C#, Performance, Bedeutung der Referenzsemantik                                                       |         TBD          | 26.01.2021 |
| Vererbung und Polymorphie                                      | Grundlagen der Vererbung, Ziele, Mehrfachvererbung, Probleme im Zusammenhang mit Vererbung, Polymorphie                                             |    Dominik Jonke     | 26.01.2021 |
| Neuerung in C# 7.0, 8.0 und 9.0                                | Tuples, Discards, Pattern Matching, Local Functions, readonly Members, Nullable Reference Types, Asynchronous Streams, etc.                         |  Michael Kienberger  | 02.02.2021 |
| Interfaces                                                     | Ziele und Einsatzgebiete von Interfaces, Contract-First-Design, Beispiele aus dem .NET-Framework                                                    |    Simon Aichmayr    | 02.02.2021 |
| ASP.NET Core - Razor Pages                                     | Grundlegende Architektur, Routing, Details Page-Models, View (Razor, PartialView, _Layout …) und Model, dynamische Datentypen (ViewBag), Tag-Helper |         TBD          | 02.02.2021 |
| Repository- und UnitOfWork-Pattern                             | Einsatzgebiet, Vorteile, Dependency Injection, Singleton, Generische Repositories                                                                   |         TBD          | 09.02.2021 |
| REST-Services mit Web-API                                      | Grundarchitektur im Zusammenhang mit ASP.NET Core, REST-Grundlagen, Entwickeln und Debuggen, Hosting                                                | Michael Gutenbrunner | 09.02.2021 |
| Events und Delegates                                           | Anwendungsgebiete bzw. Ziele, Unterschiede, Beispiele, Best Practices, etc.                                                                         |         TBD          | 09.02.2021 |
| LINQ                                                           | Zugrundeliegendes Muster, Verwendung, Operatoren, Materialisierungszeitpunkt                                                                        |     Kolev Zvonko     | 23.02.2021 |
| EF CodeFirst - Überblick                                       | Architektur, Varianten, CodeFirst Grundlagen (Convention over Configuration, Annotationen, Relationen, Vererbung,  Concurrency, ...)                |      Oscar Yim       | 23.02.2021 |
| EF CodeFirst - Validation                                      | Arten der Validierung, Auslösen und Verarbeiten von Validierungsfehlern, System.Annotation-Namespace, Benutzerdefinierte Validierungsattribute      |   Janine Höllhuber   | 23.02.2021 |
| WPF Grundlagen                                                 | Ziele, Unterschiede zu WinForms, Überblick XAML, Markup-Extensions, Dependency-Property, Attached-Property, Eventhandling                           |         TBD          | 02.03.2021 |
| WPF-Layouts und Controls                                       | Verschiedene Layouts, Beispiele, Best-Practices                                                                                                     |     Rene Lochner     | 02.03.2021 |
| WPF-DataBinding                                                | XAML-Expressions, Trigger, Binding per Code, Datenkonversation, Collection-Binding                                                                  |    Marcel Flieger    | 02.03.2021 |
| MVVM-Pattern                                                   | Allgemeine Beschreibung, Ziele, Konkrete Umsetzung in WPF                                                                                           |   Nico Fahrngruber   | 02.03.2021 |
| Continuous Integration & Continuous Delivery (Azure)           | Grundkonzept, Beispiele per Azure Devops, yaml-Syntax, Build-Pipelines, Release-Pipelines                                                           |         TBD          | 09.03.2021 |
| ASP.NET Core - Authentication / Authorization                  | Grundlegende Architektur, Authentication vs. Authorization, vorgefertigte Building Blocks, Json Web Token, Microsoft Identitiy, Identity Server     |         TBD          | 09.03.2021 |
| Reflection                                                     | Grundkonzepte, Einsatzmöglichkeiten, dynamisches Laden, Runtime Infos, etc.                                                                         |   Manuel Mairinger   | 16.03.2021 |
| Serialisierung / Deserialisierung (JSON)                       | Einsatzgebiete und Verwendung, Unterschiede Json.NET und System.Text.Json                                                                           |         TBD          | 16.03.2021 |
| Multithreading                                                 | Grundlagen, Ziele, Umsetzung mit Thread und Runnable, Erweiterte Konzepte (BackgroundWorker, AsyncTask, …), Neuerungen in .NET 4                    |         TBD          | 23.03.2021 |

## Ressourcen

* [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [GitHub Tutorial (Branches, Pullrequests, etc.)](https://guides.github.com/activities/hello-world)