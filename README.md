# REST-API


# INHALTSVERZEICHNIS

- Definition
- Prinzipien
- Aufbau und Umsetzung
- Vorteile einer REST - API
- DEMO WLP Webshop

# DEFINITION

REST-API steht für „Representational State Transfer - Application Programming Interface“. REST-API ist eine standardisierte Schnittstelle zwischen zwei unabhängigen Systemen und dient zum Datenaustausch. Durch die Nutzungen von Client - Server Architekturen oder mobilen Geräten gibt es mittlerweile zahlreiche Programmierschnittstellen (APIs), deren Aufgabe darin besteht, Webdienste nutzbar zu machen. REST ist das meist verwendete API-Konzept und kann man auch als das Programmierparadigma des Internets bezeichnet werden. Eine REST-API nutzt HTTP-Anfragen, um mittels PUT-, GET-, POST- und DELETE- Requests auf Daten zuzugreifen.

Die Kernidee bei REST ist das Konzept der Ressource, wobei eine „Ressource“ als so genannter Medientyp abgebildet wird, sozusagen die Repräsentation der Ressource ist. Alles was in REST adressierbar ist, ist eine Ressource wie z. B. der Tweet eines Twitter-Nutzers.

# REST-API PRINZIPIEN

Der Architektur-Stil verweist auf sechs Eigenschaften, die ein Dienst haben muss. Dabei ist nicht festgelegt, wie diese Prinzipien implementiert werden müssen.

- Client Server
  - Das Client-Server-Modell beschreibt eine Möglichkeit, Aufgaben und Dienstleistungen innerhalb eines Netzwerkes zu verteilen. Die Aufgaben werden von Programmen erledigt, die in Clients und Server unterteilt werden. Der Client kann auf Wunsch einen Dienst vom Server anfordern. Der Server, der sich auf demselben oder einem anderen Rechner im Netzwerk befindet, beantwortet die Anforderung. Üblicherweise arbeitet ein Server gleichzeitig für mehrere Clients.

- Zustandslosigkeit
  - Jede REST-Nachricht enthält alle Informationen, die für den Server bzw. Client notwendig sind, um die Nachricht zu verstehen. Weder der Server noch die Anwendung soll Zustandsinformationen zwischen zwei Nachrichten speichern. Man spricht daher von einem zustandslosen Protokoll. Jede Anfrage eines Clients an den Server ist insofern in sich geschlossen, als dass sie sämtliche Informationen über den Anwendungszustand beinhaltet, die vom Server für die Verarbeitung der Anfrage benötigt werden. 
  Um dennoch Zustandsinformationen auf der Client-Seite zu speichern, werden von den meisten HTTP-Anwendungen Coockies oder gleichartige Techniken verwendet.

- Caching
  - HTTP Caching ist eine Technik im HTTP-Protokoll, um Ressourcen wie Dokumente, Bilder oder Dateien in einem Cache am Client zwischenzuspeichern, um unnötige Datenübertragungen, Serveranfragen zu vermeiden und Zugriffszeiten zu verringern.

- Einheitliche Schnittstelle
  - Dies ist das Hauptunterscheidungsmerkmal von allen weiteren Architekturstilen. Ziel ist die Einheitlichkeit der Schnittstelle und somit ihre einfache Nutzung.

- Mehrschichtige Systeme
  - Die Systeme sollen mehrschichtig aufgebaut sein. Dadurch reicht es, dem Anwender lediglich eine Schnittstelle anzubieten. Dahinterliegende Ebenen können verborgen bleiben und somit die Architektur insgesamt vereinfacht werden.


- Code on Demand
  - Code on Demand, die vielleicht am wenigsten bekannte der sechs Beschränkungen und die einzige optionale Einschränkung, ermöglicht die Übertragung von ausführbaren Programmcode über die API zur Verwendung innerhalb der Anwendung am Client. Im Wesentlichen schafft sie eine intelligente Anwendung, die nicht mehr nur von ihrer eigenen Code-Struktur abhängig ist.

# AUFBAU

  ![alt text](https://api.zestard.com/wp-content/uploads/2015/12/What-is-Rest-API-02-1.jpg "Logo Title Text 1")

# 1. RESSOURCEN

Eine Ressource kann jedes Objekt sein, über das die API Informationen anbieten kann. Im Fall einer Twitter-API kann eine Ressource beispielsweise ein Benutzer, ein Hashtag oder ein beliebiger Medientyp wie ein Bild sein. Jede Ressource verfügt über eine eindeutige Kennung, die ein Name oder eine Nummer sein kann.
Die Ressource ist die Hauptabstraktion von Informationen in REST. Die Adressierbarkeit spielt dabei die Hautrolle.
Jede Ressource muss sich mit Hilfe eines eindeutigen Unique Ressource Identifiers (kurz URI) identifizieren lassen. So lässt sich z. B. in REST eine Bestellung mit der Bestellnummer 12345 mithilfe der URI http://ws.meinedomain.tld/orders/12345 adressieren.

# 2. DEFINIERTE API NORM

Die Normen der API muss selbst definiert werden.
Das heißt, die Datentransferobjekte müssen in der Clientapplikation mit der Namensgebung der REST-API am Server übereinstimmen.

BSP:

DTO Clientapplikation Angular

```typescript
export class userDto
{
  userName!: string;
  firstName!: string;
  lastName!: string;
}
```

DTO API C#

```csharp
public class UserDto
{
  public string UserName {get; set;}
  public string FirstName {get; set;}
  public string LastName {get; set;}
}
```

# 3. CLIENT

  - Der Request kann von unterschiedlichsten Clients generiert werden. So kann eine Angluar Applikation einen Request über eine Login API von Microsoft absetzen 
oder eine WPF Anwendung auf eine SMS-API zugreifen. Die jeweilige Applikation generiert den Request an den Webserver mit der entsprechenden HTTP-Methode.
Zu den Requestdaten wird ein Header angefügt, welcher Metadaten beinhaltet.
Dieser beinhaltet unter anderem folgende Inforamtionen:
    - Authorization --> beinhaltet den Token für die Authorizierung 
    - Accept --> Spezifiziert den Request bzw. Response Type. BSP. "application/json".
    - Client-request-ID --> Eindeutige Kennung des Requestes zur Nachverfolgung und Netzwerkanalyse
    - IF-Match --> Der If-Match-HTTP-Anforderungsheader macht die Anforderung abhängig. Bei GET- und HEAD-Methoden sendet der Server die angeforderte Ressource nur zurück, wenn sie mit einem der aufgelisteten ETags (Tag zur Vermeidung von Änderungen und Redundanz) übereinstimmt. Bei PUT und anderen nicht sicheren Methoden wird die Ressource nur in diesem Fall hochgeladen.

  - Die Responsedaten werden vom Server erstellt und werden auch inkl. Header zurückgeschickt.
Der Responseheader besteht aus folgenden Informationen:
    - Accept --> Gleiche Funktionsweise wie beim Request
    - Request-Id --> Ist eine eindeutige Kennung für den Anruf, mit der die ID des Requestes sichergestellt wird. Im Falle einer Zeitüberschreitung sollte der Wiederholungsaufruf denselben Wert enthalten. Nach Erhalt einer Antwort sollte der Wert für den nächsten Anruf zurückgesetzt werden.
    - Client-Request-ID --> Gleiche Funktionsweise wie beim Request
    - x-ms-ags-diagnostic --> Diagnoseinformationen vom Serverdienst
    - Timestamp --> Zeitstempel, wann der Request eingetroffen ist
    - ETag --> Tag zur Vermeidung von Änderungen und Redundanz. (Request wird unter Übermittlung verändert)

## CONTENT - TYPE

### Was ist der Content Type?
  - Der Content Type, auch MIME-Type genannt, ist eine Angabe im Response Header des Webservers, in dem einem Browser mitgeteilt wird, um was für einen Typ Ressource es sich bei der übertragenen Datei handelt. Damit kann der Browser schneller arbeiten, da der Typ der Datei nicht erst, nach Ankunft der Datei, identifiziert werden muss.

### Welche Content Typen gibt es?
  - Es gibt eine Vielzahl an verschiedenen Content Typen, die alle dazu dienen, eine übermittelte Ressource zu kennzeichnen.

  - Der MIME-Type wird immer als eine Kombination aus zwei Informationen ausgespielt, die zum einen angibt um welchen Typ Medium es sich handelt und dann, was für ein Unter-Typ dieses Mediums es ist. Schematisch sieht das dann so aus:

medientyp/subtyp

  ![alt text](https://www.runoob.com/wp-content/uploads/2014/06/F7E193D6-3C08-4B97-BAF2-FF340DAA5C6E.jpg "Logo Title Text 1")


### TEXT
Hierbei geht es um Textdateien, wobei text/html für Webseiten elementar ist. Als Subtypen gibt es zB noch text/css und text/javascript oder auch text/xml uvm.

### IMAGE
Mit dem image MIME-Type kann ein Webseitenbetreiber übermittelte Bilder als JPEG (image/jpeg) oder auch PNG (image/png) kennzeichnen, sowie SVG Dateien (image/svg+xml) und lustige Gifs von Katzen (image/gif) einbinden.
Eine spannende Neuerung der letzten Jahre ist der Typ image/webp welcher Web-optimierte statische und bewegte Bilder spezifiziert. WebP ist vor allem aus dem Grund spannend, dass dieses Dateiformat sehr kleine Dateigrößen hat und damit auch auf mobilen Endgerägen bandbreiteschonend zum Einsatz kommen kann.

### VIDEO
Bei den Videos liegt die wichtigste Unterscheidung wahrscheinlich zwischen video/mpeg und video/avi.

### AUDIO
Bei den Audiodateien wird audio/mpeg häufig noch bei gestreamter Musik genutzt.

### APPLICATION
Die Information die über application kommuniziert wird, sagt dem Browser dass es sich um Dateien handelt die mit einem bestimmten Programm geöffnet werden sollen. Der wahrscheinlich interessanteste Aspekt für das Web ist wohl application/javascript. Dies weist darauf hin, dass hier eine Serverseitige Javascriptdatei ausgeführt werden soll.
Youtube nutzt diesen MIME-Type zum Beispiel für die Videoseiten, da dort ein JavaScript Video-Player eingebunden ist, welcher sich dann um das Laden und Abspielen der eigentlichen Videodatei kümmert.
   
## GET 

Ressource wird vom Server angefordert. Serverzustand wird nicht verändert.
Folgendes Beispiel zeigt einen GET Request auf die URL: https://api.predic8.de/shop/products/1

```csharp
GET /shop/products/1 HTTP/1.1
Host: api.predic8.de
Content-Type: application/json
```

Der Response liefert das Produkt mit der ID 1 als JSON-Format.

```csharp
HTTP/1.1 200 Ok
Content-Type: application/json

{
  "id": 1,
  "name": "Wildberries",
  "price": 4.99,
  "category_url": "/shop/categories/Fruits",
  "vendor_url": "/shop/vendors/672"
}
```

## POST

Fügt eine neue Ressource hinzu. Kann auch für eine Operation verwendet werden, die von keiner anderen Methode abgedeckt wird.
Das folgende Code Beispiel zeigt das HTTP Protokoll für den Aufruf. Die Daten für die neu anzulegende Ressource wird im Body der HTTP Anfrage übertragen.
https://api.predic8.de/shop/products/

```csharp
POST /shop/products/ HTTP/1.1
Host: api.predic8.de
Content-Type: application/json

{
  "name": "Wildberries",
  "price": 4.99,
  "category_url": "/shop/categories/Fruits",
  "vendor_url": "/shop/vendors/672"
}
```

Ein Status Code von 201 Created in der Antwort unten informiert den Client über die erfolgreiche Ausführung des Requestes:

```csharp
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://api.predic8.de/shop/products/140
```

## PUT 

Die angegebene Ressource wird angelegt. Wenn die Ressource bereits existiert, wird sie geändert.
Mit PUT wird für gewöhnlich eine Ressource mit der Represäntation im Request überschrieben.

```csharp
PUT /shop/products/11 HTTP/1.1
Host: api.predic8.de
Content-Type: application/json

{
  "name": "Red Grapes",
  "price": 1.79,
  "category_url": "/shop/categories/Fruits",
  "vendor_url": "/shop/vendors/501"
}
```
Bei Erfolg antwortet der Server mit dem Status Code 200 OK.

```csharp
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

## PATCH

Nur ein Teil der Ressource wird geändert.
Mit der HTTP Methode PATCH können einzelne Eigenschaften einer Ressource gezielt manipuliert werden. Im Beispiel unten enthält der Request einen neuen Wert für die Eigenschaft price.

```csharp
PATCH /shop/products/70 HTTP/1.1
Host: api.predic8.de
Content-Type: application/json

{
  "price": 1.99
}
```

Der Body der Response enthält die Repräsentation der geänderten Ressource mit dem neuen Preis.

```csharp
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Type: application/json; charset=utf-8

{
  "name": "Pears",
  "price": 1.99,
  "photo_url": "/shop/products/70/photo",
  "category_url": "/shop/categories/Fruits",
  "vendor_url": "/shop/vendors/501"
}
```

## DELETE 

Mit der Delete Methode können Ressourcen wieder gelöscht werden.

```csharp
DELETE /shop/products/142 HTTP/1.1
Host: api.predic8.de
```

Das erfolgreiche Löschen teilt der Server mit einem Status Code 200 OK an den Client mit.

```csharp
HTTP/1.1 200 OK
```

## HEAD 

Fordert Metadaten zu einer Ressource an.

```csharp
HEAD /shop/products/142 HTTP/1.1
Accept: application/json
Host: api.predic8.de
```

HEAD Response

```csharp
HTTP/1.1 200 OK
Content-Length: 19
Content-Type: application/json //--> METADATEN ZU PRODUKT MIT DER ID 142
```

## OPTIONS

Prüft, welche Methoden auf einer Ressource zur Verfügung stehen.

```csharp
OPTIONS /shop/products HTTP/1.1
Host: api.predic8.de
```

OPTIONS Response

```csharp
HTTP/1.1 200 OK
Allow: GET,POST,PUT,PATCH,DELETE,HEAD,OPTIONS
Access-Control-Allow-Origin: https://predic8.de
Access-Control-Allow-Methods: GET,POST,PUT,PATCH,DELETE,HEAD,OPTIONS
Access-Control-Allow-Headers: Content-Type
```

## CONNECT

Dient dazu, die Anfrage durch einen TCP-Tunnel zu leiten. 
Wird meist eingesetzt, um eine HTTPS-Verbindung über einen HTTP-Proxy herzustellen.

## TRACE

Gibt die Anfrage zurück, wie sie der Zielserver erhält. Dient dazu, um Änderungen der Anfrage durch Proxyserver zu ermitteln.

# 4. REST SERVER

Der Webserver ist komplett unabhängig vom Client, empfängt die HTTP Requests und enthält die vom Client gewünschten Ressourcen.
Nach Erhalt der Anfrage wird diese vom Webserver verarbeitet, indem er die gewünschten Aktionen unter Sicherheitsanforderungen umsetzt.
Der Server gewährt nur einen Repräsentationsstatus der Quelle und keinen vollständigen Zugriff auf den Client.

Ein gutes Beispiel hierfür ist, wenn eine mobile App YouTube-Videos über eine eigene Oberfläche anzeigt. Es verwendet eine REST-API, um den Videoinhalt von YouTube aufzurufen, ohne ihn tatsächlich auf einem eigenen System zu hosten.

# VORTEILE REST API

## Skalierbarkeit

  - Die REST-API bietet eine hervorragende Skalierbarkeit. Da die Clients und Server voneinander getrennt sind, kann das Produkt vom Entwicklerteam problemlos skaliert werden.

## Integrität

  - Es ist möglich, REST in vorhandene Websites zu integrieren, ohne die Website-Infrastruktur umzugestalten. So können Entwickler schneller arbeiten, anstatt eine Website von Grund auf neu zu überarbeiten. Alternativ können sie lediglich zusätzliche Funktionen hinzufügen.

## Flexibilität

  - Entwickler können ohne Probleme auf andere Server migrieren oder Änderungen in der Datenbank vornehmen, vorausgesetzt, die Daten werden von jeder Anfrage korrekt gesendet. Die Trennung erhöht somit insgesamt die Flexibilität bei der Entwicklung.

## Unabhängigkeit

  - Dank der Trennung zwischen Client und Server können mit dem REST-Protokoll Entwicklungen in den verschiedenen Bereichen eines Projekts problemlos autonom durchgeführt werden. Darüber hinaus ist die REST-API an die betriebliche Syntax und Plattform anpassbar und bietet die Möglichkeit, zahlreiche Umgebungen während der Entwicklung zu testen.

## Asynchrone Webservices

  - Für bestimmte Webservices ist es wünschenswert, dass Anfrage und Antwort zeitlich entkoppelt sind. Da HTTP für diesen Fall keinerlei Mechanismen bietet, bleibt einzig die Option die Abarbeitungen der Anfragen als Serviceanbieter selbst zu verwalten und auf gezielte Anfrage der Nutzer weiterzuleiten.
Dies kann mit einer REST - API reibungslos implementiert werden.

# NACHTEILE EINER REST-API

Keine Standardisierung der Schnittstelle.
Namensgebung kommt von Entwickler.

# IMPLEMENTIERUNG SERVER REST-API

## PROJEKTINFORMATION

Um eine REST-API in ASP.net Core zu implementieren, wird ein Ressourcenprojekt und ein ASP.NET Core Web Projekt.
Bei Neuerstellung wird initial die Grundtruktur der API angelegt.
Gehostet wird das Projekt am localem Webserver. 127.0.0.1 localhost.

### HOSTING

  - Die fertig implementierte REST API muss über einen Web Service online zur Verfügung gestellt werden.
Diese Web Services können von vielen Anbietern herangezogen werden.

### AZURE

  - Azure Web Service ist eine Hosting Plattform von Microsoft und bietet zahlreiche Dienste an.
Dort ist es möglich einen App-Service zu "mieten" und die REST - API gegen Entgeld zu hosten.

### AZURE PIPELINES

  - Azure Pipelines sind Tools von Mircosoft, um die REST API direkt aus dem Visual Studio zu releasen.
Bei einem Commit auf den definierten Git-Branch wird die implementierte Pipeline angestoßen.
Dort werden definierte Tasks ausgeführt, um das API Projekt zu releasen.
Mehr dazu in der live Demo.

### DEBUGGING

  - Um Fehler im Source Code zu finden, kann man über die standard Debug Funktion im Visual Studio debuggen.
Um Netzwerktransaktionen zu analysieren und Verbindungsfehler ausfindig zu machen, kann man im Browser auf die Entwicklertools zurückgreifen.
Dort findet man die Requests und Response und die damit verbunden Overhead Daten, so wie Header und HTTP Status Codes.
Swagger ist auch ein hilfreiches Tool um Bugs in der REST API zu finden.

## API - Controller

API Controller sind die jeweiligen HTTP Methoden, die nach Außen für eine Ressource zur Verfügung stehen.
Ein Controller muss daher auch zugang zu der jeweiligen Ressource haben. Dies erfolgt meist über das UnitOfWork Pattern.
Jeder Controller erbt von der ControllerBase Klasse und besteht aus folgendem Aufbau:

## ERWEITERUNGEN
Es werden folgende Nugget Packeges für eine WEB API in ASP.net CORE benötigt.
- Swashbuckle.AspNetCore
- Newtonsoft.Json
- Microsoft.AspNetCore.Mvc.NewtonsoftJson

## START UP KLASSE
In der Startup Klasse wird die API konfgiguriert.
Über die ConfigureServices Methode können diverse Dienste konfiguriert werden. Unter anderem UnitOfWork, Authentifizierung, Blob Service, Cors, usw.

## ROUTING 
Durch das Route Attribut erreicht der jeweilige Request die gewünschte Ressource und die in der Header angeführte HTTP - Methode.
Der nach außen sichtbare Controller Name stammt vom Controller Klassenname.

```csharp
    [Route("api/[controller]")]
    [ApiController]
    public class UserController : ControllerBase
    {
```

Controller Klasse --> UserController
Controller Name --> User

Die Route 

```csharp
"www.easyTicket.com/api/user" 
```
verweist auf den Controller und die Ressource von USER. 
Die jeweilige Methode wird im Request Header festgelegt. 

## HTTP Attributes
Diese geben an welche HTTP Status Codes vom Controller evaluiert werden können und um welche HTTP-Methode es sich handelt.
```csharp
        [HttpGet]
        [ProducesResponseType(StatusCodes.Status200OK)]
        [ProducesResponseType(StatusCodes.Status404NotFound)]
```

## Response Methoden
Erstellen den jeweiligen HTTP Response inkl. Ressourcedaten, sofern implementiert.

BSP:
- CreatedAtAction() --> 201 Status Code
- BadRequest() --> 400 Status Code
- NotFound() --> 404 Not Found
- https://http.cat/

## Beispiel ASP .NET API Controller

```csharp
    [Route("api/[controller]")]
    [ApiController]
    public class UserController : ControllerBase
    {
        private readonly IUnitOfWork _unitOfWork;

        public UserController(IUnitOfWork unitOfWork)
        {
            this._unitOfWork = unitOfWork;
        }


        [HttpGet]
        [ProducesResponseType(StatusCodes.Status200OK)]
        [ProducesResponseType(StatusCodes.Status404NotFound)]
        public async Task<IActionResult> Get()
        {
            var users = await _unitOfWork
                .UserRepository
                .GetAllAsync();

            if (users != null)
            {
                return Ok(users);
            }
            else
            {
                return NotFound();
            }
        }

        [HttpGet("{id}")]
        [ProducesResponseType(StatusCodes.Status200OK)]
        [ProducesResponseType(StatusCodes.Status404NotFound)]
        public async Task<IActionResult> GetById(int? id)
        {
            if (!id.HasValue)
            {
                return BadRequest();
            }

            var user = await _unitOfWork.UserRepository.GetByIdAsync(id.Value);

            if (user == null)
            {
                return NotFound(new { message = "Dieser User existiert nicht!" });
            }

            return Ok(user);
        }

        
        [HttpPost]
        [ProducesResponseType(StatusCodes.Status201Created)]
        [ProducesResponseType(StatusCodes.Status400BadRequest)]
        public async Task<IActionResult> Post(UserPostDto userDto)
        {
            try
            {
                User newUser = new User(userDto)

                await _unitOfWork.UserRepository.AddAsync(newUser);
                await _unitOfWork.SaveChangesAsync();

                return CreatedAtAction(
                    nameof(Get),
                    new { id = newUser.Id },
                    userWithRoleIdDto);
            }
            catch (ValidationException validationException)
            {
                ValidationResult valResult = validationException.ValidationResult;
                ModelState.AddModelError("User erstellen Fehler", valResult.ErrorMessage);
                return BadRequest(new { message = valResult.ErrorMessage });
            }
        }
    }
}

```

## SWAGGER

Swagger ist eine Sammlung von Open-Source-Werkzeugen, um HTTP-Webservices, auch HTTP API oder REST-like API, zu entwerfen, zu erstellen, zu dokumentieren und zu nutzen.
Für die Entwicklung von APIs ist eine ordentliche, verständliche Dokumentation essenziell. Nur mit ihr können die Schnittstellen von Entwicklern eingesetzt werden.
Swagger ist die derzeit beste Möglichkeit, REST-APIs zu dokumentieren, da es nahezu alle Webservices und Informationen rund um die Schnittstelle abbilden kann. Es wächst gleichmäßig mit dem System und dokumentiert Änderungen automatisch. Das funktioniert so gut, weil Swagger die Dokumentation des REST-API direkt am Code hinterlegt.
Swagger wird in der Startup Klasse konfiguriert und stellt alle Eigenschaften der implementierten REST API da.

### Startup

```csharp
public void ConfigureServices(IServiceCollection services)
{
            services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo { Title = "Projektname.Api", Version = "v1", });
            });
}
            
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
            app.UseSwagger();
            app.UseSwaggerUI(c =>
            {
                c.SwaggerEndpoint("/swagger/v1/swagger.json", "EasyTicket API V1");
            });
}

```

Erreichbar ist Swagger unter der Route http://url/swagger/index.html


 ![alt text](https://www.baeldung.com/wp-content/uploads/2019/11/1-swagger-ui.png "Logo Title Text 1")
 ![alt text](https://www.baeldung.com/wp-content/uploads/2019/11/2-swagger-ui-api-details.png "Logo Title Text 1")

# LIVE DEMO

## VISUAL STUDIO
### ASP.NET CORE WEB APPLICATION
### NUGGET PACKAGES 
### RESSOURCE PROJEKT
### CONTROLLER
### SWAGGER
## DEBUGGING
## AZURE WEB APP
## AZURE PIPELINES

# FAZIT

Die REST-Architektur liefert hervorragende Mittel zur Konzipierung und Umsetzung von Webservices verschiedenster Art. Dank der Tatsache, dass nahezu alle Geräte das Hypertext Transfer Protocol unterstützen, können sowohl Desktop- als auch mobile Clients mühelos und ohne zusätzliche Implementierungen mit dem REST-Interface arbeiten. Das Ergebnis sind Webservices, die durch ein hohes Maß an

- Plattformunabhängigkeit,
- Skalierbarkeit,
- Performance,
- Interoperabilität
- und Flexibilität

überzeugen. 
Die Nutzung erfordert jedoch auch das entsprechende Know-how, wobei vor allem die Interaktion zwischen den einzelnen, zustandslosen Ressourcen sehr komplex und schwer zu bewerkstelligen ist.