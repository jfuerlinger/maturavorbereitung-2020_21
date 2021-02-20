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
 **_Layout.cshtml**
Das _Layout.cshtml File fungiert als Template für alle content-pages die es referenzieren.  Es beinhalten normalerweise header, footer, navigation und so weiter. Typischerweise beinhaltet das _Layout.cshtml im \<head> auch globalen Styling-Referenzén (CSS). Möchte man also den Style der Anwendung anpassen, so muss man oft nur im_Layout.cshtml Anpassungen vornehmen. 
**_ViewStart.cshtml**
Beinhaltet Code, der nach jedem Code in sämtlichen content-pages im selben Ordner oder in Subordnern ausgeführt wird. 
**_ViewImports.cshtml**
Stellt Directives global zur Verfügung, in dem das _ViewImports.cshtml File im root-Folder der Anwendung platziert wird. Alle Razor-Pages im Projekt, die hierachisch darunterliegen haben auf den Inhalt Zugriff.
**[Partial Pages](https://www.learnrazorpages.com/razor-pages/partial-pages)**
Partial Pages oder Views sind Razor-Files die teile von HTML und server-side code beinhalten. Sie werden benutzt, um komplexe Sachverhalte aufzuteilen und vorallem um in Teams die Aufgabenteilung zu erleichtern. (Work units)
__________
## Grundlegende Architektur
Die Grundlegende Razor Pages Architektur lässt sich wie folgt beschreiben:
Jede Seite (Page) unserer Website ist durch zwei Files repräsentiert
~~~
-   A  _view page_, die Informationen darstellt, und
-   A  _page model_, welches Zugriff und Verarbeitung der Informationen handhabt.
~~~
Beispiel: 
Wir erstellen eine Website für eine Bäckerei die Online Bestellungen verarbeiten soll. Wenn ein User die Website besucht, sieht er eine Seite die ein Menü und ein Formular zur Bestellung beinhaltet. Die Darstellung des Menüs und des Formulars wird in der view-page definiert. Wie das Menü beim Navigieren und das Formular beim Eingeben der Daten reagiert, wird im zugehörigen page-model definiert.

![Auszug "Solution".Web Project Architecture](images\architecture.PNG)

## Routing
Routing ist das System, das dafür zuständig ist, URL's den zugehörigen Razorpages zuzuordnen. Wie die meisten "pagecentric" Frameworks ist das Routingsystem in ASP.NET Razor Pages darauf aufgebaut URL's File-Paths zuzuorden, ausgehend vom Root Ordner. (Siehe "Pages" Ordner im Image oberhalb)

### Wie werden URL's zugewiesen?
Wenn eine Razor Pages Application gestartet wird, wird eine Collection von "Attribute Routes" generiert. Dabei werden die File und Folder Pfade im Pages folder als Basis für das Template verwendet.
~~~
Das Standard RazorPages Template besteht aus drei Paegs im root Folder (Pages):
- Error.cshtml
- Index.cshtml
- Privacy.cshtml
~~~

Index.cshtml ist standartmäßig als "default-page" festgelegt, hat also zwei Routen definiert: Einmal als empty string nach der URL und einmal mit "Index" ohne postfix ".cshtml"
~~~
Beispiel URL's:
- http://yourdomain.com/
- http://yourdomain.com/index

Diese beiden Routen sind auf den selben virtuellen Pfad gemappt: /<root>/Test/Index.cshtml
~~~

### Areas
Areas wurden in APS.NET Core 2.1 zu Razor Pages integriert. Routen, die in gemeinsamen Areas definiert sind beinhalten den Namen der Area als erstes Segment der URL.

~~~
Areas
    Adminstration
        Pages
            Index.cshtml
            Reports.cshtml
    Production
        Pages
            Index.cshtml
Pages
    Error.cshtml
    Index.cshtml
    Privacy.cshtml
~~~

Die generierten Routen für den Content in den Areas sehen wie folgt aus:
~~~
"Adminstration"
"Administration/Index"
"Administration/Reports"
"Production"
"Production/Index"
~~~

### Den Default-Root Ordner ändern
Es gibt zwei Varianten den Razor Pages Root Ordner zu ändern:
1. Configuration:
~~~
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
    .AddRazorPagesOptions(options => {
        options.RootDirectory = "/Content";
    });
}
~~~
2. WithRazorPagesRoot extension-Methode
~~~
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages().WithRazorPagesRoot("/Content");
}
~~~
### Route Templates
Route Data Parameter werden in einem Route Template festgelegt. Dieses Template ist ein Teil des @page Direktivs im .cshtml file. 
~~~
@page "{title}"
~~~
Das vorangegangene Beispiel arbeitet als Platzhalter. Die Template Definition muss in Quotes gesetzt sein, und der Parameter muss in geschwungenen Klammern angegeben werden.
In diesem Beispiel *muss* ein Wert angegeben werden. Um den Parameter optional zu kennzeichnen kann er mit einem "?" erweitert werden.
~~~
@page "{title?}"
~~~
Außerdem kann ein Default Wert angegeben werden:
~~~
@page "{title=first post}"
~~~
Es können auch mehrere Parameter angegeben werden:
~~~
@page "{year}/{month}/{day}/{title}"
~~~

Es gibt jedoch 5 Keywords die nicht als route-names verwendet werden dürfen:
~~~
- action
- area
- controller
- handler
- page
~~~

### Zugriff auf Route-Parameter
Die Route-Parameter werden in einem RouteValueDictionary gespeichert und sind durch RouteData.values zugreifbar. Man kann die Parameter folgenderweise referenzieren:
~~~
@RouteData.Values["title"]
~~~

Ein potenzielles Problem dieser Variante ist, dass es auf String referenzen basiert. Diese (durch den User eingegebenen) Strings sind anfällig für typographische Fehler, welche in Runtime-Errors resultieren. Die empfohlene Alternative wäre es, die Property-Values an ein PageModel zu "binden". Um dies zu tun, kann ein public Property im entsprechenden PageModel angelegt werden, auf das über die OnGet() Methode zugegriffen werden kann.
~~~
public class PostModel : PageModel
{
    public string Title { get; set; }
    public void OnGet(string title)
    {
        Title = title;
    }
}
~~~

Man kann den Parameter Value dem Public Property zuweisen, womit man es im Model Property auf der content-page zugreifbar macht:
~~~
@page "{title?}"
@model PostModel
@{
}
<h2>@Model.Title</h2>
~~~
Der Hauptgrund für diesen Ansatz ist, dass man so strong-typing sicherstellt und somit Intellisense support sicherstellt.

Alternativ kann man das \[BindProperty\] Attribut auf das PageModel Property mit SupportsGet = true setzen:
~~~
 public class PostModel : PageModel
{
    [BindProperty(SupportsGet = true)]
    public string Title { get; set; }
    public void OnGet()
    {
        // the Title property is automatically bound
    }
}
~~~

### Constraints hinzufügen
Man kann den Route Parametern auch Constraints zuweisen, die den Datentyp bzw. eine eventuelle Range definieren:
~~~
@page "{id:int}"
@page "{latitude:double?}"
@page "{id:min(10000)}"
@page "{username:alpha:minlength(5):maxlength(8)}"
~~~

### Override Routes
Man kann Routen overriden, d.h.: Eine Route unabhängig des Pfades definieren. Das ist nützlich, wenn ein File tief in Unterordnern des Projekts liegt, aber eine einfachere URL erhalten soll. Das ganze kann man im Template festlegen:
~~~
Pfad im Projekt: Pages/Projects/Building/SOP/Schools/Intro.cshtml
--> @page "/schools/sop"
~~~
### Weitere Routing Optionen

| Property  | Type  |  Description | 
|:-:|:-:|:-:|
| AppendTrailingSlash  |  bool | Appends a trailing slash to URLs generated by the anchor tag helper or UrlHelper. Default is false
  |
| ConstraintMap  | IDictionary<string, Type>	  | Enables the registration of custom constraints via the Add method
  |
|  LowercaseUrls |  bool |  URLs are generated all in lower case. The default is false
 |
| LowercaseQueryStrings  | bool  |  Query strings are generated all in lower case. The default is false. Will only take effect if LowercaseUrls is also true
 |
## Details Page-Models
Der Grundgedanke der PageModel Klasse ist es, eine klare Trennung zwischen UI Layer und Logic Layer zu gewährleisten. Es gibt einige Gründe dafür:
~~~
- Es reduziert die Komplexität des UI Layers, also sorgt für einfachere Wartbarkeit
- Es ermöglicht automatisiertes unit testing
- Es ermöglicht größere Flexibilität im Bezug auf Teamarbeit. (Arbeitsteilung UI & Logic)
- Es ermöglicht größere Flexibilität im Bezug auf Erweiterungen des Projekts
~~~
Aufgrund der Features und Funktionalität, ist die PageModel Klasse eine Kombination aus einem Controller und einem ViewModel:

### Controllers
Das Page Controller Pattern ist ein eins-zu-eins mapping zwischen Pages und deren Controllern. Wie vorher schon erwähnt hat also eine Page immer einen Controller, und umgekehrt.
### View Models
In RazorPages, das PageModel ist ebenso ein ViewModel. Deshalb wird bei RazorPages oft als MVVM Pattern beschrieben. (Model View ViewModel) 

## Tag Helper
Tag Helper sind wiederverwendbare Komponenten um HTML in RazorPages automatisch zu generieren. Tag Helpers beziehen sich dabei auf spezifische HTML Tags, im folgenden die meist genutzten.

- Anchor tag helper
- Cache tag helper
- Environment tag helper
- Form Action tag helper
- Form tag helper
- Image tag helper
- Input tag helper
- Label tag helper
- Link tag helper
- Option tag helper
- Partial tag helper
- Script tag helper
- Select tag helper
- Textarea tag helper
- Validation tag helper
- Validation Summary tag helper-

### Anchor tag helper
Der Anchor tag helper bezieht sich auf den AnchorTag in HTML (<a>) welcher genutzt wird um Links in HTML darzustellen.
### Cache tag helper
Der Cache tag helper erlaubt es Regionen einer RazorPage im Speicher des Servers zu cachen. Diese Funktion wird hauptsächlich benutzt um die Performance der Website zu erhöhen.
### Environment tag helper
Der Environment tag helper erlaubt es, verschiedene Inhalte abhängig von deren aktuellen Werten zu render und wird genutzt, um CSS oder JS Files zu inkludieren.
### Form Action tag helper
Der Form Action tag helper erlaubt es ein "formaction" attribut einem Element hinzuzufügen, und wird bei Buttons und Inputs verwendet.
### Form Tag Helper
Der Form Tag Helper rendert ein "action" Attribut innerhalb eines Form Elements.
### Image Tag Helper
Der Image Tag Helper bezieht sich auf das <img> Element in HTML und erlaubt die Versionierung von Image-Files.
### Input Tag Helper
Der Input Tag Helper generiert "name" und "id" Attribute basierend auf das PageModel Property dem er zugewiesen ist. Außerdem unterstützt dieser somit client-sided validation.
### Label Tag Helper
Der Label Tag Helper generiert passende "for" Attribut Values basierend auf dem PageModel Property dem er zugewiesen ist. Er wird in Verbindung mit dem Input Tag Helper verwendet.
### Link Tag Helper
Der Link Tag Helper wird benutzt um links dynamisch zu den CSS Files zu generieren, beziehungsweise um fallbacks zu generieren falls das Zielfile nicht erreichbar ist. (z.B.: wenn das Zielfile remote gelagert ist und aus irgendeinem Grund nicht verfügbar ist)
### Options Tag Helper
Der Options Tag Helper wird in Verbindung mit dem Select Tag Helper verwendet und hat zwei Verwendungszwecke:
~~~
1. Er erlaubt es manuell items zu einer Liste von Optionen hinzuzufügen die gerendert werden.

2. Falls eine der Option Values, die manuell hinzugefügt wurden dem Select Tag Helper "for" Attribut gleichen, wird dieser als "selected" gesetzt.
~~~
### Partial Tag Helper
Der Partial Tag Helper wird benutzt um die Html.Partial und Html.RenderPartial Methoden zu ersetzen und Partial Pages einzubinden.
### Script Tag Helper
Die Aufgabe des Script Tag Helpers ist es, links dynamisch zu den Script Files und Fallbacks zu generieren.
### Select Tag Helper
Die Aufgabe des Select Tag Helpers ist es, HTML "select" Elemente mit den generierten Optionen der SelectListItem Obejcts zu rendern, welche über den Options Tag Helper definiert wurden.
### Textarea Tag Helper
Die Aufgabe des Textarea Tag Helpers ist es, "textarea" Elemente zu rendern um MultiLine Text aufnehmen zu könnnen.
### Validation Tag Helper
Der Validation Tag Helper bezieht sich auf das "span" Element in HTML und wird benutzt, um Property-spezifische validation-error-messages zu rendern.
### Validation Summary Tag Helper
Der Validation Summary Tag Helper bezieht sich auf das "div" Element in HTML und wird benutzt, um eine Zusammenfassung von Form-validation errors zu rendern.

