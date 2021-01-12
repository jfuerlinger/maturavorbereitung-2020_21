# Interfaces

### Inhaltsverzeichnis:

- Definition
- Ziele
- Einsatzgebiete
- Vertrag
- Namenskonvention
- Technische Details
- Contract-First Design
- Beispiele aus dem .Net-Framework
- Vor- & Nachteile
- IEnumerable & IEnumeration
- Referenzen


### Definition

Schnittstellen sind wie eine Vertragsvereinbarung. Sobald eine Klasse eine Schnittstelle implementiert, hat der auf ein Objekt dieser Klasse zugreifende Code die Garantie, dass die Klasse die Member der Schnittstelle aufweist. Mit anderen Worten: Eine Schnittstelle legt einen Vertragsrahmen fest, den die implementierende Klasse erfüllen muss.

### Interfacedefinition

Interfaces können:
- Methoden
- Eigenschaften (Properties)
vorschreiben. Schnittstellen enthalten selbst keine Codeimplementierung, sondern nur abstrakte Definitionen. Schauen wir uns dazu eine einfache, fiktive Schnittstelle an:
---
public interface ICopy 
{
  string Caption {get; set;};
  void Copy();
}
---


### Ziele

- Interfaces trennen den Entwurf von der Implementierung

- Interfaces legen Funktionalitat fest, ohne auf die Implementierung einzugehen
  
- Beim Implementieren der Klasse ist die spatere Verwendung nicht von Bedeutung, sondern nur die
  bereitzustellende Funktionalitat
  
- Ein Anwender (eine andere Klasse) interessiert sich nicht
  für die Implementierungsdetails, sondern für die Funktionalitat

### Einsatzgebiete



### Vertrag


### Namenkonvention


### Technische Details


### Contract-First Design


### Beispiele aus dem .Net-Framework


### Vor- & Nachteile


### Referenzen
