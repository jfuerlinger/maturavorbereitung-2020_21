# *Razor Pages ASP.NET Core*

## What are Razor Pages?
Razor Pages sind eine alternative zum klassischen [MVC](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-5.0) (Model View Controller) - Pattern wenn es darum geht, serverseitige, page-focused Web-Applikationen zu erstellen. In junger Vergangenheit haben sich Razorpages in dem Bereich als Standard etabliert.
## Why not MVC?
Das Razor Pages Framework verzichtet auf viele unnötige Zeremonien die nötig sind, um eine Anwendung in MVC zu erstellen, ist simpler aufgebaut und somit auch besser wartbar und übersichtlicher.
Komponenten im MVC Framework haben immernoch ihren Nutzen, zum Beispiel wenn man Controller für RESTful APIs braucht.
## How does it work?
Alle Razor-Files enden mit *.cshtml*. Die meisten dieser Files enthalten eine Mischung aus client- und serversided code, wenn dieser verarbeitet wird, entsteht HTML-Code der an den Browser geschickt wird.
Diese Pages werden normalerweise "content pages" genannt, die im folgenden genauer beschrieben werden.
### [Content Pages](https://www.learnrazorpages.com/razor-pages)
Damit sich ein File als Content-Page verhält, sind 3 Eigenschaften unverzichtbar:
 - Es darf nicht mit einem "leading-underscore" benannt sein: ~~*_razorpage.cshtml*~~
 - Das File hat die Endung *.cshtml*
 - Das erste Zeile des Files startet mit *@page* (ähnliches verhalten .xml header)
### [Different types of Razor files](https://www.learnrazorpages.com/razor-pages/files/)
Andere Razor Files haben einen "leading underscore" im Namen, welcher häufig dafür benutzt wird um partial pages zu benennen. Die drei Folgenden Files haben aber bestimmte Funktionen innerhalb der Razor Pages Applikation.
- **_Layout.cshtml**
Das _Layout.cshtml File fungiert als Template für alle content-pages die es referenzieren.  Es beinhalten normalerweise header, footer, navigation und so weiter. Typischerweise beinhaltet das _Layout.cshtml im \<head> auch globalen Styling-Referenzén (CSS). Möchte man also den Style der Anwendung anpassen, so muss man oft nur im_Layout.cshtml Anpassungen vornehmen. 
- **_ViewStart.cshtml**
Beinhaltet Code, der nach jedem Code in sämtlichen content-pages im selben Ordner oder in Subordnern ausgeführt wird. 
- **_ViewImports.cshtml**
Stellt Directives global zur Verfügung, in dem das _ViewImports.cshtml File im root-Folder der Anwendung platziert wird. Alle Razor-Pages im Projekt, die hierachisch darunterliegen haben auf den Inhalt Zugriff.
- **[Partial Pages](https://www.learnrazorpages.com/razor-pages/partial-pages)**
Partial Pages oder Views sind Razor-Files die teile von HTML und server-side code beinhalten. Sie werden benutzt, um komplexe Sachverhalte aufzuteilen und vorallem um in Teams die Aufgabenteilung zu erleichtern. (Work units)
____________________
### [PageModel](https://www.learnrazorpages.com/razor-pages/pagemodel)
Die Razor Pages PageModel Klasse hat die Hauptaufgabe, den UI-Layer und die dahinterstehende Logik aufzuteilen, was folgende Vorteile bietet:
- Es reduziert die Komplexität des UI Layers
- Es ermöglicht automatisiertes UnitTesting
- Es erlaubt eine größere Flexibilität im Bezug auf Arbeit in Teams
- Es ermöglicht es, kleine, wiederverwendbare "Units of Code" auszukapseln und diese wo anders zu benutzen.
### Tag Helpers
//todo 
### Routing
//todo 
### Configuration
//todo
### Dependency Injection
//todo
### Validation
//todo
### Model Binding
//todo
### State Management
//todo
### Working with Json
//todo

