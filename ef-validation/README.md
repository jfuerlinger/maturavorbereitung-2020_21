# Entity Framework Validation

## Validierung allgemein 

* Zur Überprüfung der Daten auf Korrektheit

* Selbes Konzept für mehrere Ebenen:
    * WPF
    * Web-UI clientseitig 
    * Entity direkt
    * Persistenzschicht
    * Domainspezifisch
    * Datenbankabhängig
    * Validierung im Browser
    * Validierung am Desktop

* Validation-Framework:
    * Ermöglicht Validierung in allen Bereichen
    * ValidationExecption zum Transport von Exceptions 
    * Kernklasse Validator (statisch):
        * ValidateObject 
        * TryValidateObject
        * ValidateProperty
    
## Möglichkeiten
* DataAnnotations für einzelne Properties:
    * MaxLength, RegEx, EmailAddress, ...
    * CustomValidation
    * Eigene Annotationsklassen


Beispiel für Vordefinierte Annotations:
![](VordefinierteAnnotations.png)

Beispiel Custom Validation:
![](CustomValidation.png)
![](CustomValidation2.png)

## Datenbank

* Die Prüfung erfrodert den Zugriff auf die Datenbank
* Prüfmethode benötigt IUnitOfWork
* Aufruf der Validierung über SaveChanges-Methode

Beispiel Datenbank Validation:
![](DatenbankValidation.png)
![](DatenbankValidation2.png)

## WPF

* Um die WPF Validierung nutzen zu können benötigt man im zugehörigen ViewModel das Interface INotifyDataErrorInfo.

* HasErrors:

* ErrorsChanged:
* GetErrors

[Zur Übersicht](../README.md)