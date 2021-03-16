# WPF Databinding


## Inhaltsverzeichnis
1. Definition
2. Basic data binding concepts
3. Richtung des data flows
4. Synchronisation
5. Data binding XAML
6. Data binding code behind
7. INotifyPropertyChanged
8. Data conversion

## Definition

Data binding ist der Prozess, der die Verbindung zwischen einem UI-Element und den Daten, die diese anzeigt, herstellt.
Wenn die Daten, gegen welche gebunden wurden, sich geändert haben, werden diese auch automatisch im UI-Element aktualisiert. Hierbei können die Daten auch von außerhalb in der UI durch den
Benutzer verändert werden, dies ändert ein Property im ViewModel, welches dann beim Anstoss des Speicherns durch den Code in der DB gespeichert wird. 


## Basic data binding concepts

Egal welches Element gebunden wird, alle Bindings folgen dem folgenden Model:
![](https://github.com/mflieger/testreadme/blob/main/basic-data-binding-diagram.png "Basic Data Binding Diagram")

Wie das Model zeigt, bildet data binding die Brücke zwischen dem binding target und der binding source.



Jedes binding besteht aus 4 Komponenten:
- binding target object
- target property
- binding source
- Einen Pfad zum Wert, den die binding source verwenden kann

z.B. Wenn man den Inhalt einer TextBox an einen Employee.Name binden möchte, so ist das target object die TextBox, target property ist die text property,
der zu verwendende Wert ist Name und das source object ist das Employee object.


- Jedes target property muss ein dependency property sein. Die meisten UI Elemente properties sind dependency properties und bis auf read-only properties unterstützen
die meisten auch data binding.


## Richtung des data flows

Der data flow beim data binding kann in eine Richting, als wie auch in beide Richtungen gehen.
Somit kann man kontrollieren, ob der Benutzer Daten im source verändern kann oder nicht.

![](https://github.com/mflieger/testreadme/blob/main/databinding-dataflow.png "dataflow")

- OneWay bindings sorgen dafür, dass Veränderungen im source property auch am target property aktualisiert werden. Dies ist nützlich für read-only properties.
- TwoWay bindings sorgen dafür, dass Veränderungen in beiden Richtungen aktualisiert werden. Besonders hilfreich für editierbaren Formularen oder interaktiven UI's.
Die meisten properties sind TwoWay bindings bei default.
- OneWayToSource ist genau das Gegenteil vom OneWay binding, bei dem Änderungen in der UI im Source gespeichert werden. 
- OneTime beschreibt nur eine einzige Veränderung die von der source im target property aktualisiert wird.
- Default ist der Standardmodus. Ist meist ein TwoWay binding.


## Synchronisation

Updatesourcetriggers:
- Explicit: Die Änderung muss explizit durch den Aufruf der UpdateSource-Methode des Zielobjekts erfolgen.
- LostFocus: Die source wird aktualisiert, wenn die Zielkomponente den Fokus verliert.
- PropertyChanged: Die Aktualisierung erfolgt bei jeder Änderung. Allerdings beansprucht diese Einstellung die Ressourcen sehr intensiv.
- Default: Der default variiert von Komponente zu Komponente, ist allerdings meist ein PropertyChanged und in selten Fällen ein LostFocus.


## Binding erstellen mit XAML

```xaml
<Window x:Class="HelloBinding!.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="100" Width="250">
    <StackPanel Margin="10">
        <TextBox Name="txtValue" />
        <WrapPanel Margin="0,10">
            <TextBlock Text="Value: " FontWeight="Bold" />
            <TextBlock Text="{Binding Path=Text, ElementName=txtValue}" />
        </WrapPanel>
    </StackPanel>
</Window>

```

![](https://github.com/mflieger/testreadme/blob/main/Hello!.PNG "Hello!")

In diesem Beispiel zeigen wir mit der Verknüpfung den Wert einer TextBox, in einer anderen TextBox an. Ohne data binding müssten wir dafür ein event erzeugen.

### Binding Syntax

- {Binding Path = NameDerEigenschaft}
Path-Element beschreibt die Eigenschaft, zu der man die Verknüpfung realisieren möchte.

- {Binding Path=Text, ElementName=txtValue}
Eine Verknüpfung kann durch verschiedene Eigenschaften erfolgen, unter Anderem die Eigenschaft ElementName, welche wir in unserem Beispiel verwendeten. 
Dies gibt uns die Möglichkeit direkt an andere UI-Elemente als source zu verknüpfen. Jede Eigenschaft, die wir im Binding setzen, wird mit einem Komma (',') getrennt.

## Data binding code behind

- Data binding sollte eigentlich ausschließlich im XAML definiert werden. Man kann es allerdings auch im code behind definieren.

```xaml
<Window x:Class="WpfTutorialSamples.DataBinding.CodeBehindBindingsSample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="CodeBehindBindingsSample" Height="110" Width="280">
    <StackPanel Margin="10">
    <TextBox Name="txtValue" />
    <WrapPanel Margin="0,10">
        <TextBlock Text="Value: " FontWeight="Bold" />
        <TextBlock Name="lblValue" />
    </WrapPanel>
    </StackPanel>
</Window>
```

```C#
using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;

namespace WpfTutorialSamples.DataBinding
{
    public partial class CodeBehindBindingsSample : Window
    {
    public CodeBehindBindingsSample()
    {
        InitializeComponent();

        Binding binding = new Binding("Text");
        binding.Source = txtValue;
        lblValue.SetBinding(TextBlock.TextProperty, binding);
    }
    }
}
```

Wir erstellen eine binding instance und geben im constructor den Pfad an ("Text"), geben als nächstes die source an (txtValue) und in der letzten Zeile fügen wir unser binding object 
mit dem target property zusammen. SetBinding Methode benötigt hierbei 2 Parameter: an welches dependency property wir binden möchten und einen, der das binding object enthält.

## INotifyPropertyChanged

Objects die OneWay or TwoWay bindings nutzen, müssen das 'INotifyPropertyChanged' interface implementieren. 

Beispiel:

```C#
class Person : INotifyPropertyChanged
{
	private string _firstName;
	public string FirstName
	{
		get { return _firstName; }
		set { _firstName = value; OnPropertyChanged(nameof(FirstName)); }
	}
	
	public event PropertyChangedEventhHandler PropertyChanged;
	private void OnPropertyChanged(string propertyName)
	{
		PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
	}
}
```

## Data conversion

```xaml
<Window x:Class="WpfTutorialSamples.DataBinding.ConverterSample"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
		xmlns:local="clr-namespace:WpfTutorialSamples.DataBinding"
        Title="ConverterSample" Height="140" Width="250">
	<Window.Resources>
		<local:YesNoToBooleanConverter x:Key="YesNoToBooleanConverter" />
	</Window.Resources>
	<StackPanel Margin="10">
		<TextBox Name="txtValue" />
		<WrapPanel Margin="0,10">
			<TextBlock Text="Current value is: " />
			<TextBlock Text="{Binding ElementName=txtValue, Path=Text, Converter={StaticResource YesNoToBooleanConverter}}"></TextBlock>
		</WrapPanel>
		<CheckBox IsChecked="{Binding ElementName=txtValue, Path=Text, Converter={StaticResource YesNoToBooleanConverter}}" Content="Yes" />
	</StackPanel>
</Window>
```


```C#
using System;
using System.Windows;
using System.Windows.Data;

namespace WpfTutorialSamples.DataBinding
{
	public partial class ConverterSample : Window
	{
		public ConverterSample()
		{
			InitializeComponent();
		}
	}

	public class YesNoToBooleanConverter : IValueConverter
	{
		public object Convert(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
		{
			switch(value.ToString().ToLower())
			{
				case "yes":
					return true;
				case "no":
					return false;
			}
			return false;
		}

		public object ConvertBack(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
		{
			if(value is bool)
			{
				if((bool)value == true)
					return "yes";
				else
					return "no";
			}
			return "no";
		}
	}
}
```


Wir haben einen Konverter, in der die Convert()-Methode davon ausgeht, dass sie einen String als Eingabe (den value-Parameter) erhält
und diesen dann in einen booleschen wahren oder falschen Wert mit einem Ersatz-Wert von false umwandelt. 

Die ConvertBack Methode macht das Gegenteil: Er nimmt einen Eingabewert mit einem booleschen Typ an und gibt dann das englische Wort "yes" oder "no" zurück,
mit einem Ersatz-Wert von "no".
