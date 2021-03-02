# Events und Delegates


## �bersicht
* Delegates
* Eigenschaften von Delegates
* Events
* EventHandler
* Event data
* Beispiel


### Delegates

Ein Delegate ist ein Typ, der Verweise auf Methoden mit einer bestimmten Parameterliste und dem R�ckgabetyp darstellt. Nach der Instanziierung kann ein Delegate mit jeder belieben Methode verkn�pft werden, die eine kompatible Methoden-Signatur aufweist. Bei der Methode kann es sich um eine statische Methode oder um eine Instanzenmethode handeln. Diese Flexibilit�t erm�glicht das programmgesteuerte �ndern von Methodenaufrufen oder die Integration von neuem Code in bereits vorhandene Klassen.
Delegates werden verwendet, um Methoden als Argumente an andere Methoden zu �bergeben. Ereignishandler sind Methoden, die durch Delegates aufgerufen werden.

Deklaration eines Delegates:
~~~cs
public delegate int PerformCalculation(int x, int y);
~~~

### Eigenschaften von Delegates
* sind Zeiger auf eine Methode und sind vollst�ndig objektorientiert.
* kapseln sowohl eine Objektinstanz als auch eine Methode.
* erm�glichen es Methoden als Parameter zu �bergeben.
* k�nnen zum Definieren von R�ckrufmethoden verwendet werden.
* k�nnen miteinander verkettet werden (Es k�nnen mehrere Methoden f�r ein einziges Event aufgerufen werden).



### Events
Events basieren auf dem Delegate-Modell. Im Kontext der Events ist ein Delegat ein Mittler (ein zeiger�hnlicher Mechanismus) zwischen der Ereignisquelle und dem Code, mit dem das Ereignis behandelt wird.

Ein Event ist eine Meldung, die von einem Objekt gesendet wird und signalisiert das etwas passiert ist. Dies kann durch den Benutzer ausgel�st werden (z.B.: Button-Click) oder auch durch andere Programmteile (z.B.: ein Wert �ndert sich). Das Objekt das das Event ausl�st  ist der "Event Sender" und diesem ist nicht bekannt welche Methode/Objekt das Event empfangen wird. Das Ereignis ist in der Regel ein Member des Ereignissenders. Beispielsweise ist das Click-Event ein Member der Klasse Button.



### EventHandler

Deklaration eines EventHandlers:
~~~cs
public event EventHandler<T> TresholdReached;
~~~

Die Methode zum Ausl�sen des Events wird wie folgt deklariert:
~~~cs 
protected virtual void OnThresholdReached(EventArgs e)
{
    EventHandler handler = ThresholdReached;
    handler?.Invoke(this, e);
}
~~~

Um auf ein Event zu reagieren, definiert man eine EventHandler-Methode im Ereignisempf�nger. Diese Methode muss der Signatur des Delegates f�r das behandelte Event entsprechen.Um bei Auftreten des Ereignisses Benachrichtigungen zu empfangen, muss die EventHandler-Methode das Ereignis abonnieren.

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

Die EventArgs-Klasse ist der Basistyp aller Ereignisdatenklassen. EventArgs ist auch die Klasse, die bei einem Event verwendet wird, dem keine Daten zugeordnet sind. Will man nun eine andere Klasse �ber ein Event benachrichtigen ohne das Daten gesendet werden verwendet man "EventArgs.Empty". 

Um eine benutzerdefinierte Ereignisdatenklasse zu erstellen, erstellt man eine Klasse, welche von EventArgs ableitet und alle Member bereit stellt die f�r das damit verkn�pfte Event erforderlich sind. 
Wichtig! In .Net enden Ereignisdaten-Klassennamen auf "EventArgs".
 
Im folgenden Beispiel wird eine Ereignisdatenklasse ThresholdReachedEventArgs veranschaulicht. Darin sind Eigenschaften enthalten, die f�r das ausgel�ste Event spezifisch sind.
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

[Zur �bersicht](../README.md)