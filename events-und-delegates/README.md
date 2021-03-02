# Events und Delegates


## Übersicht
* Delegates
* Eigenschaften von Delegates
* Events
* EventHandler
* Event data
* Beispiel


### Delegates

Ein Delegate ist ein Typ, der Verweise auf Methoden mit einer bestimmten Parameterliste und dem Rückgabetyp darstellt. Nach der Instanziierung kann ein Delegate mit jeder belieben Methode verknüpft werden, die eine kompatible Methoden-Signatur aufweist. Bei der Methode kann es sich um eine statische Methode oder um eine Instanzenmethode handeln. Diese Flexibilität ermöglicht das programmgesteuerte Ändern von Methodenaufrufen oder die Integration von neuem Code in bereits vorhandene Klassen.
Delegates werden verwendet, um Methoden als Argumente an andere Methoden zu übergeben. Ereignishandler sind Methoden, die durch Delegates aufgerufen werden.

Deklaration eines Delegates:
~~~cs
public delegate int PerformCalculation(int x, int y);
~~~

### Eigenschaften von Delegates
* sind Zeiger auf eine Methode und sind vollständig objektorientiert.
* kapseln sowohl eine Objektinstanz als auch eine Methode.
* ermöglichen es Methoden als Parameter zu übergeben.
* können zum Definieren von Rückrufmethoden verwendet werden.
* können miteinander verkettet werden (Es können mehrere Methoden für ein einziges Event aufgerufen werden).



### Events
Events basieren auf dem Delegate-Modell. Im Kontext der Events ist ein Delegat ein Mittler (ein zeigerähnlicher Mechanismus) zwischen der Ereignisquelle und dem Code, mit dem das Ereignis behandelt wird.

Ein Event ist eine Meldung, die von einem Objekt gesendet wird und signalisiert das etwas passiert ist. Dies kann durch den Benutzer ausgelöst werden (z.B.: Button-Click) oder auch durch andere Programmteile (z.B.: ein Wert ändert sich). Das Objekt das das Event auslöst  ist der "Event Sender" und diesem ist nicht bekannt welche Methode/Objekt das Event empfangen wird. Das Ereignis ist in der Regel ein Member des Ereignissenders. Beispielsweise ist das Click-Event ein Member der Klasse Button.



### EventHandler

Deklaration eines EventHandlers:
~~~cs
public event EventHandler<T> TresholdReached;
~~~

Die Methode zum Auslösen des Events wird wie folgt deklariert:
~~~cs 
protected virtual void OnThresholdReached(EventArgs e)
{
    EventHandler handler = ThresholdReached;
    handler?.Invoke(this, e);
}
~~~

Um auf ein Event zu reagieren, definiert man eine EventHandler-Methode im Ereignisempfänger. Diese Methode muss der Signatur des Delegates für das behandelte Event entsprechen.Um bei Auftreten des Ereignisses Benachrichtigungen zu empfangen, muss die EventHandler-Methode das Ereignis abonnieren.

~~~cs
class Program
{
    static void Main()
    {
        var c = new Counter();
        //Subscribe
        c.ThresholdReached += c_ThresholdReached;

        //Unsubscribe
        //c.ThresholdReached -= c_ThresholdReached;
        

        // provide remaining implementation for the class
    }

    static void c_ThresholdReached(object sender, EventArgs e)
    {
        Console.WriteLine("The threshold was reached.");
    }
}
~~~


### Ereignisdaten/Event data
Ereignisdaten sind Daten die einem Event zugewiesen sind und durch eine Ereignisdatenklasse bereitgestellt werden. 

Die EventArgs-Klasse ist der Basistyp aller Ereignisdatenklassen. EventArgs ist auch die Klasse, die bei einem Event verwendet wird, dem keine Daten zugeordnet sind. Will man nun eine andere Klasse über ein Event benachrichtigen ohne das Daten gesendet werden verwendet man "EventArgs.Empty". 

Um eine benutzerdefinierte Ereignisdatenklasse zu erstellen, erstellt man eine Klasse, welche von EventArgs ableitet und alle Member bereit stellt die für das damit verknüpfte Event erforderlich sind. 
Wichtig! In .Net enden Ereignisdaten-Klassennamen auf "EventArgs".
 
Im folgenden Beispiel wird eine Ereignisdatenklasse ThresholdReachedEventArgs veranschaulicht. Darin sind Eigenschaften enthalten, die für das ausgelöste Event spezifisch sind.
~~~cs
public class ThresholdReachedEventArgs : EventArgs
{
    public int Threshold { get; set; }
    public DateTime TimeReached { get; set; }
} 
~~~


### Beispiel

~~~cs
namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            Counter c = new Counter(new Random().Next(10));
            c.ThresholdReached += c_ThresholdReached;

            Console.WriteLine("press 'a' key to increase total");
            while (Console.ReadKey(true).KeyChar == 'a')
            {
                Console.WriteLine("adding one");
                c.Add(1);
            }
        }

        static void c_ThresholdReached(object sender, ThresholdReachedEventArgs e)
        {
            Console.WriteLine("The threshold of {0} was reached at {1}.", e.Threshold,  e.TimeReached);
            Environment.Exit(0);
        }
    }

    class Counter
    {
        private int threshold;
        private int total;

        public event EventHandler<ThresholdReachedEventArgs> ThresholdReached;

        public Counter(int passedThreshold)
        {
            threshold = passedThreshold;
        }

        public void Add(int x)
        {
            total += x;
            if (total >= threshold)
            {
                ThresholdReachedEventArgs args = new ThresholdReachedEventArgs();
                args.Threshold = threshold;
                args.TimeReached = DateTime.Now;
                OnThresholdReached(args);
            }
        }

        protected virtual void OnThresholdReached(ThresholdReachedEventArgs e)
        {
            EventHandler<ThresholdReachedEventArgs> handler = ThresholdReached;
            if (handler != null)
            {
                handler(this, e);
            }
        }
    }

    public class ThresholdReachedEventArgs : EventArgs
    {
        public int Threshold { get; set; }
        public DateTime TimeReached { get; set; }
    }
}
~~~

[Zur Übersicht](../README.md)