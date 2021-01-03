# MVVM-Pattern

## Was ist MVVM?
Das MVVM-Pattern (Model View ViewModel Muster) wurde erstmals 2005 von Microsoft entwickelt und veröffentlicht und sollte ursprünglich nur in WPF-Anwendungen eingesetzt werden. Inzwischen wird es, ähnlich wie MVC, in anderen Bereichen ebenfalls eingesetzt.

MVVM ist ein Entwurfsmuster und eine Variante des MVC-Musters (Model-View-Controller). Das MVVM-Muster dient zur Trennung der Logik von der grafischen Benutzeroberfläche und findet Einsatz in WPF, JavaFX, HTML5 und Xamarin.Forms. 

![alt text](mvvm.png)

MVVM kann man entweder selbst implementieren oder man greift auf MVVM-Frameworks zurück, wie etwa Prism oder MVVM-Light.

## Verwendung
MVVM enthält folgende drei Komponenten:

* Model: Das Model ist die Datenzugriffsschicht auf die Inhalte, die der Benutzer manipulieren kann. Das Model benachritigt UI-Elemente wenn Daten geändert worden sind und führt Validierungen durch. Hier ist auch die gesammte Geschäftslogik enthalten.
* View: Die View ist das was der Benutzer im Programm sieht. Sie besteht aus allen grafischen Elementen die das Programm ausmachen. Die View bindet sich an Eigenschaften aus dem ViewModel um Inhalte darzustellen, zu manipulieren oder um Benutzereingaben weiterzuleiten. 
* ViewModel: Das ViewModel entählt die Logik der UI und ist das Bindeglied zwischen Model und View. Das ViewModel kann auf der einen Seite auf Methoden und Dienste des Models zugreifen und diese ausführen. Auf der anderen Seite stellt es öffentliche (public) Eigenschaften und Befehle (Commands) zur Verfügung, an welche sich die View binden kann. Das ViewModel darf jedoch keine Abhängigkeiten oder Referenzen auf die View haben. Das bedeutet auch, dass im ViewModel nicht direkt auf UI-Elemente (TextBoxen, Buttons, usw.) zugegriffen werden kann.

Nachdem für die Verwendung des MVVM-Patterns die sogenannten Datenbindung nötig ist (für die Bindung der Properties und Commands an UI-Elemente), kann MVVM nur in jenen Sprachen verwendet werden, welche einen solchen Binder-Mechanismus zu Verfügung stellen. Für .NET und JavaFX werden hierfür Views aus XAML-Markup bzw. FXML erstellt und anschließend das Databinding implementiert. Die Logik hinter der UI wird danach mit C# oder Java implementiert.

Seit 2010 gibt es MVVM auch für JavaScript bzw. HTML. Hierfür wurde der Datenbindungsmechanismus portiert, die daraus entstanden JavaScript-Bibliothek heißt Knockout.js. Mittlerweile findet MVVM auch Anwendung in anderen JavaScript-Frameworks wie zum Beispiel AngularJS. 

MVVM kann auch für die Entwicklung mobiler Apps verwendet werden. Für Android und iOS kann mit Hilfe von Xamarin.Forms ebenfalls das MVVM-Pattern implementiert werden. Die Gestaltung der UI erfolgt wie in WPF durch XAML-Markup, die Logik wird in C# geschrieben.

## Genauere Betrachtung
### Das Model

Das Model stellt eine Abbildung der Daten dar, welche für die visuelle Anwendung benötigt werden. Es enthält jene Daten, welche dem Benutzer zur Verfügung gestellt und von ihm manipuliert werden und die Geschäftslogik. 

Das Model stellt folgende Funktionalitäten bereit:

* Validierungen
* Benachrichtigungen bei Änderungen
* Verarbeitung bestimmter Regeln

Für die Validierung und die Benachrichtigung bei Änderungen, muss das Model die Interfaces INotifyPropertyChanged und IDataErrorInfo implementieren. 

Beispiel für ein Model:

```c#
public class ModelBase : INotifyPropertyChanged, IDataErrorInfo
{
    protected void OnPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
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
    private string _name;
    private string _street;
    private string _streetNumber;
    private string _city;
    private string _zip;
    private string _country;

    public string Name
    {
        get => _name;
        set
        {
            _name = value;
            OnPropertyChanged();
        }
    }

    public string Street
    {
        get => _street;
        set
        {
            _street = value;
            OnPropertyChanged();
        }
    }

    public string StreetNumber
    {
        get => _streetNumber;
        set
        {
            _streetNumber = value;
            OnPropertyChanged();
        }
    }

    public string City
    {
        get => _city;
        set
        {
            _city = value;
            OnPropertyChanged();
        }
    }

    public string Zip
    {
        get => _zip;
        set
        {
            _zip = value;
            OnPropertyChanged();
        }
    }

    public string Country
    {
        get => _country;
        set
        {
            _country = value;
            OnPropertyChanged();
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

Wie man dem Beispiel entnehmen kann, wird im Setter eines jeden Propertys die Methode OnPropertyChanged aufgerufen. Diese Methode wird bei jeder Änderung an dem jeweiligen Property aufgerufen und sendet eine Benachrichtigung aus, dass das Property geändert wurde.

### Die View 

Die View stellt, wie schon erwähnt, die grafische Benutzeroberfläche dar und enthält alle Elemente (Buttons, Listen, Grids, Inputfelder), welche für die konkrete Anwendung benötigt werden. 
Bei der Implementierung des MVVM-Patterns sollte darauf geachtet werden, dass im Code-Behind der View so wenig Logik wie möglich verwendet wird. Optimal wäre, wenn garkein Code im Code-Behind geschrieben wird, dies lässt sich aber nicht immer so umsetzen. Befindet sich Code im Code-Behind, darf sich dieser Code nur auf die View beziehen und keine zusätzlichen Abhängigkeiten mit sich bringen. 

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

### Das ViewModel

Das View Model ist das Model zur konkreten View. Auf das ViewModel wird durch Databinding gebunden und es stellt Commands zur Verfügung worauf ebenfalls gebunden werden kann. Das bedeutet zum Beispiel, dass für einen Buttonclick kein dementsprechender Event-Handler im Code-Behind der View erstellt wird, sondern dass das ClickEvent das Command im ViewModel auslöst. 
Besonders wichtig am ViewModel ist, dass keine Referenzen oder Abhängigkeiten auf die View vorhanden sein dürfen. Dies würde dem Grundprizip von MVVM, nämlich der Trennung und losen Kopplung durch Databinding, widersprechen.

Wird in der View eine Auflistung von Objekten bereit gestellt, kann man hierfür im ViewModel eine `ObservableCollection<T>` verwenden. Diese Klasse ermöglicht das Erkennen von Änderungen an der Auflistung und gibt diese Änderungen über das Binding System weiter, ohne dass dafür Code geschrieben werden muss.

Das ViewModel ist auch dafür zuständig, Daten (zum Beispiel aus einer Datenbank) zu laden. 

Beispiel für ein ViewModel

```c#
public class ViewModelBase : INotifyPropertyChanged
{
    protected void OnPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

Beispiel für eine konkrete Implementierung:

```c#
public class MainViewModel : ViewModelBase
    {
        private ObservableCollection<TeamTableRowDto> _teamTableRowDtos;
        public ObservableCollection<TeamTableRowDto> TeamTableRowDtos 
        {
            get => _teamTableRowDtos;
            set
            {
                _teamTableRowDtos = value;
                OnPropertyChanged();
            }
        }

        public MainViewModel(IWindowController windowController) : base(windowController)
        {
            LoadCommands();
        }

        public ICommand CmdNewGame { get; set; }

        /// <summary>
        /// Erstellt die notwendigen Commands.
        /// </summary>
        private void LoadCommands()
        {
            CmdNewGame = new RelayCommand(
                execute: async _ =>
                {
                    Controller.ShowWindow(await NewGameViewModel.CreateAsync(Controller), true);
                    await LoadDataAsync();
                },
                canExecute: _ => true);
        }

        /// <summary>
        /// Asynchrones Laden von Daten für das ViewModel.
        /// Wird in CreateAsync(..) aufgerufen.
        /// </summary>
        /// <returns></returns>
        private async Task LoadDataAsync()
        {
            using(UnitOfWork uow = new UnitOfWork())
            {
                TeamTableRowDtos = new ObservableCollection<TeamTableRowDto>(await uow.Teams.GetTeamTableRowDtosAsync());
            }
        }

        public static async Task<MainViewModel> CreateAsync(IWindowController windowController)
        {
            var viewModel = new MainViewModel(windowController);
            await viewModel.LoadDataAsync();
            return viewModel;
        }
    }
```

#### Commands

In MVVM wird für Commands das ICommand Interface verwendet. Dieses Interface bietet zwei Methoden an:

```c#
public interface ICommand
{
    bool CanExecute(object parameter);
    void Execute(object parameter);
    event EventHandler CanExecuteChanged;
}
```

CanExecute wird beim Laden des Fensters (WPF) oder der Seite (Xamarin.Forms) aufgerufen. Diese Methode ermittelt, ob alle Bedingungen erfüllt sind, damit das Command ausgeführt werden kann. An Hand eines Buttons könnte dies zum Beispiel dazu führen, dass der Button solange disabled (ausgegraut) ist, bis eine bestimmte Bedingung (Kein Eingabefeld darf leer sein) erfüllt ist.
Die Execute Methode ist dann die Methode, welche aufgerufen wird, sobald der Button geklickt wurde. Damit lässt sich zum Beispiel ein neues Fenster öffnen oder Daten über eine UnitOfWork/API in eine Datenbank persistieren/laden.

Für die Verwendung von Commands eignet es sich, wenn man ein RelayCommand implementiert. RelayCommands enthalten Actions und Predicates. Das sind die jeweiligen Methoden welche das Command ausführt und die Bedingung, wann das Command ausgeführt werden kann. Anstatt eine neue Klasse für jedes Command zu erstellen, welche dann das ICommand Interface implementiert, braucht man nur eine RelayCommand Klasse implementieren und im Konsturktor die jeweiligen Methoden mitgeben.

```c#
public class RelayCommand : ICommand
    {
        private readonly Action<object> _execute;
        private readonly Predicate<object> _canExecute;

        public RelayCommand(Action<object> execute, Predicate<object> canExecute)
        {
            _execute = execute;
            _canExecute = canExecute;
        }

        public bool CanExecute(object parameter)
            => _canExecute == null || _canExecute(parameter);

        public event EventHandler CanExecuteChanged
        {
            add { CommandManager.RequerySuggested += value; }
            remove { CommandManager.RequerySuggested -= value; }
        }

        public void Execute(object parameter) => _execute(parameter);
    }
```

In der Praxis:

```c#
CmdSaveGame = new RelayCommand(
                execute: async _ =>
                {
                    using(UnitOfWork uow = new UnitOfWork())
                    {
                        Game newGame = new Game()
                        {
                            Round = Round,
                            HomeTeam = await uow.Teams.GetTeamByIdAsync(SelectedHomeTeam.Id),
                            GuestTeam = await uow.Teams.GetTeamByIdAsync(SelectedGuestTeam.Id),
                            HomeGoals = HomeGoals,
                            GuestGoals = GuestGoals
                        };
                        await uow.Games.AddAsync(newGame);

                        try
                        {
                            await uow.SaveChangesAsync();
                            Controller.CloseWindow(this);
                        }
                        catch(ValidationException ex)
                        {
                            if(ex.Value is IEnumerable<string> properties)
                            {
                                foreach(var property in properties)
                                {
                                    AddErrorsToProperty(property, new List<string> { ex.ValidationResult.ErrorMessage });
                                }
                            }
                            else
                            {
                                DbError = ex.Message;
                            }
                        }
                    }
                },
                canExecute: _ => IsValid && ChangesMade());
```

Bei der Erzeugung eines RelayCommands wird, wie aus dem Code-Abschnitt ersichtlich, für execute eine Action definiert (in diesem Fall Zugriff auf eine Datenbank und Schließen des aktuellen Fensters) und für canExecute wird angegeben, welche Bedingungen erfüllt sein müssen (Eingabefelder müssen valide sein und es muss mindestens eine Änderung in einem Eingabefeld vorgenommen worden sein). Wie bei canExecute kann auch der Code im execute in eine eigene Methode ausgelagert werden.

## MVVM-Frameworks - Prism

Prism ist ein mächtiges opensource MVVM-Framework welches ursprünglich von Microsoft entwickelt wurde. Prism stellt die Implementierung einer Sammlung verschiedenster nützlicher Design-Pattern zur Verfügung, wie MVVM, Commands, Dependecy Injection, usw.. Prism wird heutzutage für die Entwicklung von WPF und Xamarin.Forms Applikationen verwendet. Im Folgenden wird kurz auf ein paar Eigenschaften und Unterschiede zur herkömmlichen MVVM-Implementierung eingegangen.

### INotifyPropertyChanged-Implementierung

Wenn man INotifyPropertyChanged in einem größeren Projekt immer und immer wieder implementieren muss, ist dies nicht nur zeitaufwändig sondern auch fehleranfällig. Prism bietet hierfür eine BindableBase Klasse, von der andere Klassen ableiten können. Diese BindableBase bietet die Methode `SetProperty<T>(ref T storage, T value)`, welche die Implementierung von INotifyPropertyChanged um einiges beschleunigt und sicherer gegenüber Fehlern macht.

```c#
public abstract class BindableBase : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    ...
    protected virtual bool SetProperty<T>(ref T storage, T value,
                            [CallerMemberName] string propertyName = null)
    {...}
    protected void OnPropertyChanged<T>(
                            Expression<Func<T>> propertyExpression)
    {...}

    protected void OnPropertyChanged(string propertyName)
    {...}
}
```

Beispielverwendung:

```c#
        private string _additionalInformation;
        private Activity _selectedActivity;
        private ObservableCollection<Activity> _activities;

        public string AdditionalInformation 
        {
            get => _additionalInformation;
            set => SetProperty(ref _additionalInformation, value);
        }
        public Activity SelectedActivity 
        {
            get => _selectedActivity;
            set => SetProperty(ref _selectedActivity, value);
        }
        public ObservableCollection<Activity> Activities 
        {
            get => _activities;
            set => SetProperty(ref _activities, value);
        }
```

Wie man sieht, kann man nun auch beim Setter eine Lambda-Expression verwenden und man erhält einen schönen Einzeiler, anstatt 3-4 Zeilen des immer gleichen Codes von `_field = value; OnPropertyChanged(nameof(_field))`.

### Commands

Das Prism-Framework bietet für die Verwendung von Commands die Klasse DelegateCommand. Wie das RelayCommand, bietet diese Klasse eine Execute und eine CanExecute Methode an. Dadurch erspart man sich die manuelle Implementierung einer RelayCommand Klasse.

```c#
public class ArticleViewModel
{
    public DelegateCommand SubmitCommand { get; private set; }

    public ArticleViewModel()
    {
        SubmitCommand = new DelegateCommand<object>(Submit, CanSubmit);
    }

    void Submit(object parameter)
    {
        //implement logic
    }

    bool CanSubmit(object parameter)
    {
        return true;
    }
}
```

### Navigation

Je nach dem auf welcher Platform Prism zum Einsatz kommt, gibt es unterschiedliche Arten der Navigation. Im Unterricht wurde die Navigation bis her immer mit einer selbst geschriebenen Controller Klasse realisiert, welche die Methoden `ShowWindow(BaseViewModel model)` und  `CloseWindow(BaseViewModel model)` besitzt. In Prism für Xamarin.Forms wird für die Navigation ein eigenes Interface zur Verfügung gestellt, INavigationService.
Dieses Interface wird per Dependency Injection im jeweiligen ViewModel geladen und enthält Methoden wie `NavigateAsync(string destination)` und `GoBackAsync()`. 

```c#
_navigationService.NavigateAsync("MainPage");
```

Im obigen Code-Snippet wird der `_navigationService` aufgerufen, welcher zuvor durch Dependecy Injection geladen wurde. Der Paramter "MainPage" ist der Name der Ziel-View, zu welcher navigiert werden soll. Ein großer Vorteil, welcher der NavigationService von Prism hat, ist, dass er automatisch versucht zu erkennen, ob eine Seite modal oder normal geöffnet werden soll. Möchte man auf Nummer sicher gehen, kann man auch explizit angeben, dass die Navigation modal erfolgen soll.

```c#
_navigationService.NavigateAsync("MainPage", useModalNavigation: true);
```

Ein erheblicher Vorteil vom NavigationService von Prism, speziell im Zusammenhang mit Xamarin.Forms, ist, dass die normale Navigation in Xamarin über den Code-Behind der Views ablaufen würde. Durch Prisms NavogationService lässt sich dieser Code-Behind komplett vermeiden und die Navigation kann alleine in den ViewModels ablaufen.

### ViewModelLocator

Eine weitere tolle Eigenschaft an Prism ist der sogenannte ViewModelLocator. Der ViewModelLocator bindet den DataContext einer View automatisch anhand der Namensgebungskonventionen an das jeweilige ViewModel. Wichtig hierfür ist, dass die Views und ViewModels in der selben Assembly sind, dass die Views in einem .Views Unterordner und die ViewModels in einem .ViewModels Unterordner sind und dass der Name der beiden (bis auf die Endung ViewModel) ident sind. Es ist jedoch auch möglich, diese Regeln zu überschreiben und eigene Regeln aufzustellen.

```c#
protected override void ConfigureViewModelLocator()
{
    base.ConfigureViewModelLocator();

    ViewModelLocationProvider.SetDefaultViewTypeToViewModelTypeResolver((viewType) =>
    {
        var viewName = viewType.FullName.Replace(".ViewModels.", ".CustomNamespace.");
        var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;
        var viewModelName = $"{viewName}ViewModel, {viewAssemblyName}";
        return Type.GetType(viewModelName);
    });
}
```

### Fazit

Auch wenn dies nur ein ganz kleiner Ausschnitt aus der Toolbox von Prism war, so sieht man doch, dass die Verwendung eines MVVM-Frameworks die Arbeit erheblich erleichtern und die Menge an Boiler-Plate-Code reduzieren kann. Hat man also vor an einem größeren Projekt das MVVM-Pattern zu implementieren, so ist es sicher eine gute Idee auf ein MVVM-Framework zurückzugreifen.

## Vor- und Nachteile von MVVM
### Vorteile

* Die Geschäftslogik kann einfach ausgetauscht werden, ohne dass Änderungen an der UI nötig sind. Diese leichte Austauschbarkeit lässt sich auf das MVC-Muster zurück führen.
* ViewModel können unabhängig von der View instanziert werden, dadurch erhöht sich die Testbarkeit. Es sind nämlich nun keine UI-Tests mehr nötig, codebasierte Tests reichen nun aus.
* Gegenüber dem MVC-Muster wird die Menge an "GlueCode" erheblich reduziert. GlueCode ist Code, welcher keine direkte Funktionalität bietet, sondern lediglich da ist, um verschieden Programmteile "zusammenzukleben".
* MVVM fördert die Aufgabentrennung bei der Entwicklung. Designer können ungestört an der UI (Views) arbeiten, während die Entwickler die Logik (Model und ViewModel) implementieren.
* Diverse MVVM-Frameworks erleichtern die Implementierung und ersparen dem Entwickler viel Boiler-Plate-Code.

### Nachteile

* Ein Nachteil des DataBindings ist ein erhöhter Rechenaufwand, da hierbei ein sogenannter bi-direktionaler (twoway) Beobachter eingesetzt wird. Nachdem das MVVM-Muster ohne DataBinding nicht auskommt, kann man dies auch als Nachteil am MVVM sehen.
* Ein weiterer Nachteil ist, dass bei kleineren Anwendungen die Implementierung des MVVM-Patterns die Komplexität der Anwendung unnötig erhöht.

[Zur Übersicht](../README.md)