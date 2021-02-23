# WPF Databinding


## Inhaltsverzeichnis
1. Definition
2. Basic data binding concepts
3. Richtung des data flows
4. Synchronisation
5. Data binding XAML
6. Data binding code behind
7. Daten konvertieren

## Definition

Data binding ist der Prozess, der die Verbindung zwischen einem UI-Element und den Daten die diese anzeigt herstellt.
Wenn Daten des Data Binding ge�ndert werden, werden diese auch automatisch im UI-Element aktualisiert. Hierbei k�nnen die Daten auch von au�erhalb in der UI durch den
Benutzer ver�ndert werden und diese werden dann in der Datenbank aktualisiert. 


## Basic data binding concepts

Egal welches Element gebindet wird, alle Bindings folgen dem folgenden Model:
![](https://github.com/mflieger/testreadme/blob/main/basic-data-binding-diagram.png "Basic Data Binding Diagram")

Wie das Model zeigt, bildet data binding die Br�cke zwischen dem binding target und der binding source.



Jedes binding besteht aus 4 Komponenten:
- binding target object
- target property
- binding source
- Einen Pfad zum Wert, den die binding source verwenden kann

z.B. Wenn man den Inhalt einer TextBox an einen Employee.Name binden m�chte, so ist das target object die TextBox, target property ist die Text property,
der zu verwendende Wert ist Name und das source object ist das Employee object.


- Jedes target property muss ein dependency property sein. Die meisten UI Elemente properties sind dependency properties und bis auf read-only properties unterst�tzen
die meisten auch data binding.


## Richtung des data flows

Der data flow beim data binding kann in eine Richting, als wie auch in beide Richtungen gehen.
Somit kann man kontrollieren, ob der Benutzer Daten im source ver�ndern zu k�nnen oder nicht.

![](https://github.com/mflieger/testreadme/blob/main/databinding-dataflow.png "dataflow")

- OneWay bindings sorgen daf�r, dass Ver�nderungen im source property auch automatisch am target property aktualisiert werden. Dies ist n�tzlich f�r read-only properties.
- TwoWay bindings sorgen daf�r, dass Ver�nderungen in beiden Richtungen aktualisiert werden. Besonders hilfreich f�r editierbaren Formularen oder interaktiven UI's.
Die meisten properties sind TwoWay bindings bei default.
- OneWayToSource ist genau das Gegenteil vom OneWay binding, bei dem �nderungen in der UI im Source gespeichert werden. 
- OneTime beschreibt nur eine einzige Ver�nderung die von der source im target property aktualisiert wird.
- Default ist der Standardmodus. Ist meist ein TwoWay binding.


## Synchronisation

Updatesourcetriggers:
- Explicit: Die �nderung muss explizit durch den Aufruf der UpdateSource-Methode des Zielobjekts erfolgen.
- LostFocus: Die source wird aktualisiert, wenn die Zielkomponente den Fokus verliert.
- PropertyChanged: Die Aktualisierung erfolgt bei jeder �nderung. Allerdings beansprucht diese Einstellung die Ressourcen sehr intensiv.
- Default: Der default variiert von Komponente zu Komponente, ist allerdings meist ein PropertyChanged und in selten F�llen ein LostFocus.


## Binding erstellen mit XAML

```xaml
<Window x:Class="WpfTutorialSamples.DataBinding.HelloBoundWorldSample"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="HelloBoundWorldSample" Height="110" Width="280">
    <StackPanel Margin="10">
		<TextBox Name="txtValue" />
		<WrapPanel Margin="0,10">
			<TextBlock Text="Value: " FontWeight="Bold" />
			<TextBlock Text="{Binding Path=Text, ElementName=txtValue}" />
		</WrapPanel>
	</StackPanel>
</Window>

```

In diesem Beispiel zeigen wir den Wert eines TextBox einer anderen TextBox durch die Verkn�pfun an. Ohne data binding m�ssten wir daf�r ein Ereignis erzeugen.

### Binding Syntax

- {Binding Path = NameDerEigenschaft}
Path-Element beschreibt die Eigenschaft, zu der man die Verkn�pfung realisieren m�chte.

- {Binding Path=Text, ElementName=txtValue}
Eine Verkn�pfung kann durch verschiedene Eigenschaften erfolgen, unter Anderem die Eigenschaft ElementName, welche wir in unserem Beispiel verwendeten. 
Dies gibt uns die M�glichkeit direkt an andere UI-Elementen als Quelle zu verkn�pfen. Jede Eigenschaft, die wir im Binding setzen, wird mit einem Komma getrennt.

## Data binding code behind

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

Wir erstellen eine Binding-Instanz und geben im Konstruktor den Pfad an ("Text"), geben als n�chstes die source an (txtValue) und in der letzten Zeile f�gen wir unser Binding-object 
mit dem Ziel-Element zusammen. SetBinding Methode ben�igt hierbei 2 Parameter: and welche Abh�ngigkeitseigenschaft wir binden m�chten und einen der das Binding-object enth�lt.

## Daten konvertieren

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


Wir haben einen Konverter in derDie Convert()-Methode geht davon aus, dass sie einen String als Eingabe (den value-Parameter) 
und diesen dann in einen booleschen wahren oder falschen Wert mit einem Ersatz-Wert von false umwandelt. 

Die ConvertBack Methode macht das Gegenteil: Er nimmt einen Eingabewert mit einem booleschen Typ an und gibt dann das englische Wort "yes" oder "no" zur�ck,
mit einem Ersatz-Wert von "no".
