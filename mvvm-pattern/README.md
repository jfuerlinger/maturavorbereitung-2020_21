# MVVM-Pattern

[Zur Übersicht](../README.md)

<h2>Was ist MVVM?</h2>
Das MVVM-Pattern (Model View ViewModel Muster) wurde erstmals 2005 von Microsoft entwickelt und veröffentlicht und sollte ursprünglich nur in WPF-Anwendungen eingesetzt werden. Inzwischen wird es, ähnlich wie MVC, in anderen Bereichen ebenfalls eingesetzt.

MVVM ist ein Entwurfsmuster und eine Variante des MVC-Musters (Model-View-Controller). Das MVVM-Muster dient zur Trennung der Logik von der grafischen Benutzeroberfläche und findet Einsatz in WPF, JavaFX, HTML5 und Xamarin.Forms. 

<h2>Verwendung</h2>
MVVM enthält folgende drei Komponenten:

* Model: Das Model ist die Datenzugriffsschicht auf die Inhalte, die der Benutzer manipulieren kann. Das Model benachritigt UI-Elemente wenn Daten geändert worden sind und führt Validierungen durch. Hier ist auch die gesammte Geschäftslogik enthalten.
* View: Die View ist das was der Benutzer im Programm sieht. Sie besteht aus allen grafischen Elementen die das Programm ausmachen. Die View bindet sich an Eigenschaften aus dem ViewModel um Inhalte darzustellen, zu manipulieren oder um Benutzereingaben weiterzuleiten. 
* ViewModel: Das ViewModel entählt die Logik der UI und ist das Bindeglied zwischen Model und View. Das ViewModel kann auf der einen Seite auf Methoden und Dienste des Models zugreifen und diese ausführen. Auf der anderen Seite stellt es öffentliche (public) Eigenschaften und Befehle (Commands) zur Verfügung, an welche sich die View binden kann. Das ViewModel darf jedoch keine Kenntnis über die View haben.

Nachdem für die Verwendung des MVVM-Patterns die sogenannten Datenbindung nötig ist (für die Bindung der Properties und Commands an UI-Elemente), kann MVVM nur in jenen Sprachen verwendet werden, welche einen solchen Binder-Mechanismus zu Verfügung stellen. Für .NET und JavaFX werden hierfür Views aus XAML-Markup bzw. FXML erstellt und anschließend das Databinding implementiert. Die Logik hinter der UI wird danach mit C# oder Java implementiert.

Seit 2010 gibt es MVVM auch für JavaScript bzw. HTML. Hierfür wurde der Datenbindungsmechanismus portiert, die daraus entstanden JavaScript-Bibliothek heit Knockout.js. Mittlerweile findet MVVM auch Anwendung in anderen JavaScript-Frameworks wie zum Beispiel AngularJS. 

MVVM kann auch für die Entwicklung mobiler Apps verwendet werden. Für Android und iOS kann mit Hilfe von Xamarin.Forms ebenfalls das MVVM-Pattern implementiert werden. Die Gestaltung der UI erfolgt wie in WPF durch XAML-Markup, die Logik wird in C# geschrieben.

<h2>Genauere Betrachtung</h2>
<h3>Das Model</h3>

Das Model stellt eine Abbildung der Daten dar, welche für die visuelle Anwendung benötigt werden. Es enthält jene Daten, welche dem Benutzer zur Verfügung gestellt und von ihm manipuliert werden. 

Das Model stellt folgende Funktionalitäten bereit:

* Validierungen
* Benachrichtigungen bei Änderungen
* Verarbeitung bestimmter Regeln

Für die Validierung und die Benachrichtigung bei Änderungen, muss das Model die Interfaces INotifyPropertyChanged und IDataErrorInfo implementieren. 

Beispiel für ein Model:

```c#
public class ModelBase : INotifyPropertyChanged, IDataErrorInfo
{
    protected void RaisePropertyChanged(string propertyName)
    {
        PropertyChangedEventHandler handler = PropertyChanged;
        if (handler != null)
            handler(this, new PropertyChangedEventArgs(propertyName));
    }

    public event PropertyChangedEventHandler PropertyChanged;

    public virtual string Error
    {
        get { return String.Empty; }
    }

    public virtual string this[string columnName]
    {
        get { return String.Empty; }
    }
}
```

Konkretes Beispiel für ein Model anhand eines Unternehmens:

```c#
public class CompanyModel : ModelBase
{
    private string name;
    private string street;
    private string streetNumber;
    private string city;
    private string zip;
    private string country;

    public string Name
    {
        get { return name; }
        set
        {
            if (name == value)
                return;
            name = value;
            RaisePropertyChanged("Name");
        }
    }

    public string Street
    {
        get { return street; }
        set
        {
            if (street == value)
                return;
            street = value;
            RaisePropertyChanged("Street");
        }
    }

    public string StreetNumber
    {
        get { return streetNumber; }
        set
        {
            if (streetNumber == value)
                return;
            streetNumber = value;
            RaisePropertyChanged("StreetNumber");
        }
    }

    public string City
    {
        get { return city; }
        set
        {
            if (city == value)
                return;
            city = value;
            RaisePropertyChanged("City");
        }
    }

    public string Zip
    {
        get { return zip; }
        set
        {
            if (zip == value)
                return;
            zip = value;
            RaisePropertyChanged("Zip");
        }
    }

    public string Country
    {
        get { return country; }
        set
        {
            if (country == value)
                return;
            country = value;
            RaisePropertyChanged("Country");
        }
    }

    public override string this[string columnName]
    {
        get
        {
            switch (columnName)
            {
                case "Name":
                    if (String.IsNullOrWhiteSpace(Name))
                        return "Name des Unternehmens muss befüllt sein";
                    break;
            }
            return null;
        }
    }
}
```

Wie man dem Beispiel entnehmen kann, wird in jedem Property die Methode RaisePropertyChanged (oft auch OnPropertyChanged genannt) aufgerufen. Diese Methode wird bei jeder Änderung an dem jeweiligen Property aufgerufen und sendet eine Benachrichtigung aus, dass das Property geändert wurde.

<h3>Die View</h3>

Die View stellt, wie schon erwähnt, die grafische Benutzeroberfläche dar und enthält alle Elemente (Buttons, Listen, Grids, Inputfelder), welche für die konkrete Anwendung benötigt werden. 
Bei der Implementierung des MVVM-Patterns soll darauf geachtet werden, dass im Code-Behind der View so wenig Logik wie möglich verwendet wird. Optimal wäre, wenn garkein Code im Code-Behind geschrieben wird, dies lässt sich aber nicht immer so umsetzen. Befindet sich Code im Code-Behind, darf sich dieser Code nur auf die View beziehen und keine zusätzlichen Abhängigkeiten mit sich bringen. 

Beispiel für eine View:

```xml
<UserControl x:Class="DevTyr.MvvmObjectsDemo.View.CompanyView"
             xmlns="schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="schemas.microsoft.com/expression/blend/2008" 
             mc:Ignorable="d" 
             d:DesignHeight="300" d:DesignWidth="300">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>    
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>

        <TextBlock Text="Name" Grid.Row="0"/>
        <TextBlock Text="Strasse" Grid.Row="1"/>
        <TextBlock Text="Hausnummer" Grid.Row="2"/>
        <TextBlock Text="Stadt" Grid.Row="3"/>
        <TextBlock Text="PLZ" Grid.Row="4"/>
        <TextBlock Text="Land" Grid.Row="5"/>

        <TextBox Text="{Binding Name}" Grid.Column="1" Grid.Row="0"/>
        <TextBox Text="{Binding Street}" Grid.Column="1" Grid.Row="1"/>
        <TextBox Text="{Binding StreetNumber}" Grid.Column="1" Grid.Row="2"/>
        <TextBox Text="{Binding City}" Grid.Column="1" Grid.Row="3"/>
        <TextBox Text="{Binding Zip}" Grid.Column="1" Grid.Row="4"/>
        <TextBox Text="{Binding Country}" Grid.Column="1" Grid.Row="5"/>
    </Grid>
</UserControl>
```

In diesem Beispiel ist der Code-Behind des XAML-Files bis auf die Initialisieirung der Komponenten im Konstruktor leer. Die Text-Properties der Textboxen werden mit Databinding und der dementsprechenden Syntax an die Properties im ViewModel gebunden. Syntax: `UI-Property = "{Binding PropertyName}"`

<h3>Das ViewModel</h3>

Das View Model ist das Model zur konkreten View. Auf das ViewModel wird durch Databinding gebunden und es stellt Commands zur Verfügung worauf ebenfalls gebunden werden kann. Das bedeutet zum Beispiel, dass für einen Buttonclick kein dementsprechendes Event im Code-Behind der View erstellt wird, sondern dass das ClickEvent durch das Command im ViewModel gesteuert wird. 
Besonders wichtig am ViewModel ist, dass keine Referenzen oder Abhängigkeiten auf die View vorhanden sein dürfen. Dies würde dem Grundprizip von MVVM, nämlich der Trennung und losen Kopplung durch Databinding, widersprechen.

Wird in der View eine Auflistung von Objekten bereit gestellt, kann man hierfür im ViewModel eine `ObservableCollection<T>` verwenden. Diese Klasse ermöglicht das Erkennen von Änderungen an der Auflistung und gibt diese Änderungen über das Binding System weiter, ohne dafür Code schreiben zu müssen.

Das ViewModel ist auch dafür zuständig, Daten (zum Beispiel aus einer Datenbank) zu laden. 

Beispiel für ein ViewModel

```c#
public class ViewModelBase : INotifyPropertyChanged
{
    protected void RaisePropertyChanged(string propertyName)
    {
        PropertyChangedEventHandler handler = PropertyChanged;
        if (handler != null)
            handler(this, new PropertyChangedEventArgs(propertyName));
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

Beispiel für eine konkrete Implementierung:

```c#
public class CompanyViewModel : ViewModelBase
{
    private CompanyModel model;

    public CompanyModel Company
    {
        get { return model; }
        set
        {
            if (model == value)
                return;
            model = value;
            RaisePropertyChanged("Company");
        }
    }
}
```

<h2>Vor- und Nachteile</h2>
<h3>Vorteile</h3>

* Die Geschäftslogik kann einfach ausgetauscht werden, ohne dass Änderungen an der UI nötig sind. Diese leichte Austauschbarkeit lässt sich auf das MVC-Muster zurück führen.
* ViewModel können unabhängig von der View instanziert werden, dadurch erhöht sich die Testbarkeit. Es sind nämlich nun keine UI-Tests mehr nötig, codebasierte Tests reichen nun aus.
* Gegenüber dem MVC-Muster wird die Menge an "GlueCode" erheblich reduziert. GlueCode ist Code, welcher keine direkte Funktionalität bietet, sondern lediglich da ist, um verschieden Programmteile "zusammenzukleben".
* MVVM fördert die Aufgabentrennung bei der Entwicklung. Designer können ungestört an der UI (Views) arbeiten, während die Entwickler die Logik (Model und ViewModel) implementieren.

<h3>Nachteile</h3>

* Die Verwendung von Datenbindung kann zu einem erhöhten Rechenaufwand führen, da hierbei ein sogenannter bi-direktionaler (twoway) Beobachter eingesetzt wird.
* 
