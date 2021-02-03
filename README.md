# REST-API


# Inhaltsverzeichnis:

- Definition
- Prinzipien
- Aufbau und Umsetzung
- Vorteile einer REST - API
- DEMO WLP Webshop

# Definition

REST-API steht für „Representational State Transfer - Application Programming Interface“. REST-API ist eine standardisierte Schnittstelle zwischen zwei unabhängigen Systemen und dient zum Datenaustausch. Durch die Nutzungen von Client - Server Architekturen oder mobilen Geräten gibt es mittlerweile zahlreiche Programmierschnittstellen (APIs), deren Aufgabe darin besteht, Webdienste nutzbar zu machen. REST ist das meist verwendete API-Konzept und kann man auch als das Programmierparadigma des Internets bezeichnet werden. Eine REST-API nutzt HTTP-Anfragen, um mittels PUT-, GET-, POST- und DELETE- Requests auf Daten zuzugreifen.

Die Kernidee bei REST ist das Konzept der Ressource, wobei eine „Ressource“ als so genannter Medientyp abgebildet wird, sozusagen die Repräsentation der Ressource ist. Alles was in REST adressierbar ist, ist eine Ressource wie z. B. der Tweet eines Twitter-Nutzers.

# REST-API Prinzipien?

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
Code on Demand, die vielleicht am wenigsten bekannte der sechs Beschränkungen und die einzige optionale Einschränkung, ermöglicht die Übertragung von Code oder Applets über die API zur Verwendung innerhalb der Anwendung. Im Wesentlichen schafft sie eine intelligente Anwendung, die nicht mehr nur von ihrer eigenen Code-Struktur abhängig ist.

# Aufbau:

  ![alt text](https://api.zestard.com/wp-content/uploads/2015/12/What-is-Rest-API-02-1.jpg "Logo Title Text 1")

# 1. Ressourcen

Eine Ressource kann jedes Objekt sein, über das die API Informationen anbieten kann. Im Fall einer Twitter-API kann eine Ressource beispielsweise ein Benutzer, ein Hashtag oder ein beliebiger Medientyp wie ein Bild sein. Jede Ressource verfügt über eine eindeutige Kennung, die ein Name oder eine Nummer sein kann.
Die Ressource ist die Hauptabstraktion von Informationen in REST. Die Adressierbarkeit spielt dabei die Hautrolle.
Jede Ressource muss sich mit Hilfe eines eindeutigen Unique Ressource Identifiers (kurz URI) identifizieren lassen. So lässt sich z. B. in REST eine Bestellung mit der Bestellnummer 12345 mithilfe der URI http://ws.meinedomain.tld/orders/12345 adressieren.

# 2. Eine definierte API Norm

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

# 3. Client 

Die Browserapplikation generiert den Request and Webserver mit der entsprechenden HTTP-Methode.
Die Requestdaten werden befinden sich im Header.
   
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

# 4. REST - Server

Der Webserver ist komplett unabhängig vom Client, empfängt die HTTP Requests und enthält die vom Client gewünschten Ressourcen.
Nach erhalt der Anfrage wird diese vom Webserver verarbeitet, indem er die gewünschten Aktionen unter Sicherheitsanforderungen umsetzt.
Der Server gewährt nur einen Repräsentationsstatus der Quelle und keinen vollständigen Zugriff auf den Client.

Ein gutes Beispiel hierfür ist, wenn eine mobile App YouTube-Videos über eine eigene Oberfläche anzeigt. Es verwendet eine REST-API, um den Videoinhalt von YouTube aufzurufen, ohne ihn tatsächlich auf einem eigenen System zu hosten.

# Vorteile einer REST API

## Skalierbarkeit

Die REST-API bietet eine hervorragende Skalierbarkeit. Da die Clients und Server voneinander getrennt sind, kann das Produkt vom Entwicklerteam problemlos skaliert werden.

## Integrität

Es ist möglich, REST in vorhandene Websites zu integrieren, ohne die Website-Infrastruktur umzugestalten. So können Entwickler schneller arbeiten, anstatt eine Website von Grund auf neu zu überarbeiten. Alternativ können sie lediglich zusätzliche Funktionen hinzufügen.

## Flexibilität

Entwickler können ohne Probleme auf andere Server migrieren oder Änderungen in der Datenbank vornehmen, vorausgesetzt, die Daten werden von jeder Anfrage korrekt gesendet. Die Trennung erhöht somit insgesamt die Flexibilität bei der Entwicklung.

## Unabhängigkeit

Dank der Trennung zwischen Client und Server können mit dem REST-Protokoll Entwicklungen in den verschiedenen Bereichen eines Projekts problemlos autonom durchgeführt werden. Darüber hinaus ist die REST-API an die betriebliche Syntax und Plattform anpassbar und bietet die Möglichkeit, zahlreiche Umgebungen während der Entwicklung zu testen.

## Asynchrone Webservices

Für bestimmte Webservices ist es wünschenswert, dass Anfrage und Antwort zeitlich entkoppelt sind. Da HTTP für diesen Fall keinerlei Mechanismen bietet, bleibt einzig die Option die Abarbeitungen der Anfragen als Serviceanbieter selbst zu verwalten und auf gezielte Anfrage der Nutzer weiterzuleiten.
Dies kann mit einer REST - API reibungslos implementiert werden.

# Nachteil einer REST API

Keine Standardisierung der Schnittstelle.
Namensgebung kommt von Entwickler.

# LIVE DEMO easyWLP Webshop

# FAZIT

Die REST-Architektur liefert hervorragende Mittel zur Konzipierung und Umsetzung von Webservices verschiedenster Art. Dank der Tatsache, dass nahezu alle Geräte das Hypertext Transfer Protocol unterstützen, können sowohl Desktop- als auch mobile Clients mühelos und ohne zusätzliche Implementierungen mit dem REST-Interface arbeiten. Das Ergebnis sind Webservices, die durch ein hohes Maß an

- Plattformunabhängigkeit,
- Skalierbarkeit,
- Performance,
- Interoperabilität
- und Flexibilität

überzeugen. 
Die Nutzung erfordert jedoch auch das entsprechende Know-how, wobei vor allem die Interaktion zwischen den einzelnen, zustandslosen Ressourcen sehr komplex und schwer zu bewerkstelligen ist.