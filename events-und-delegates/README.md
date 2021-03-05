# Events und Delegates


## �bersicht
* Delegates
* Eigenschaften von Delegates
* Verwenden von Varianz bei Delegates
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
* Methoden m�ssen nicht exakt mit dem Typ des Delegaten �bereinstimmen (Varianz in Delgates).
* Lambdaausdr�cke sind eine pr�zisere Methode zum Schreiben von Inlinecodebl�cken. Lambdaausdr�cke werden (in bestimmten Kontexten) in Delegattypen kompiliert. 


### Verwenden von Varianz bei Delegates
Wenn man einem Delegate eine Methode zuweist, bieten Kovarianz und Kontravarianz Flexibilit�t f�r das Abgleichen eines Delegattyps mit einer Methodensignatur. Kovarianz l�sst die Verf�gung einer Methode �ber einen R�ckgabetyp zu, der st�rker abgeleitet ist als der im Delegat definierte Typ. Kontravarianz l�sst eine Methode zu, die �ber Typen verf�gt, die weniger abgeleitet sind als die im Delegattyp.

**Kovarianz**
Delegates k�nnen mit Methoden verwendet werden, die �ber R�ckgabetypen verf�gen, die von den R�ckgabetypen in der Delegatsignatur abgeleitet sind. Der von DogsHandler zur�ckgegebene Datentyp ist vom Typ Dogs, der vom im Delegat definierten Typ Mammals abh�ngt.
~~~cs
class Mammals {}  
class Dogs : Mammals {}  
  
class Program  
{  
    // Define the delegate.  
    public delegate Mammals HandlerMethod();  
  
    public static Mammals MammalsHandler()  
    {  
        return null;  
    }  
  
    public static Dogs DogsHandler()  
    {  
        return null;  
    }  
  
    static void Test()  
    {  
        HandlerMethod handlerMammals = MammalsHandler;  
  
        // Covariance enables this assignment.  
        HandlerMethod handlerDogs = DogsHandler;  
    }  
}
~~~

**Kontravarianz**
In diesem Beispiel wird veranschaulicht, wie Delegaten mit Methoden verwendet werden k�nnen, die �ber Parameter eines Typs verf�gen, die Basistypen von den Parametertypen der Delegatsignatur sind. Mithilfe von Kontravarianz k�nnen Sie einen Ereignishandler anstelle getrennter Handler verwenden.

~~~cs
public delegate void KeyEventHandler(object sender, KeyEventArgs e)
public delegate void MouseEventHandler(object sender, MouseEventArgs e)
~~~
Das Beispiel definiert einen Ereignishandler mit einem EventArgs-Parameter und verwendet diesen, um die Ereignisse Button.KeyDown und Button.MouseClick zu bearbeiten. Dies ist m�glich, weil EventArgs der Basistyp sowohl von KeyEventArgs als auch von MouseEventArgs ist.

~~~cs
// Event handler that accepts a parameter of the EventArgs type.  
private void MultiHandler(object sender, System.EventArgs e)  
{  
    label1.Text = System.DateTime.Now.ToString();  
}  
  
public Form1()  
{  
    InitializeComponent();  
  
    // You can use a method that has an EventArgs parameter,  
    // although the event expects the KeyEventArgs parameter.  
    this.button1.KeyDown += this.MultiHandler;  
  
    // You can use the same method
    // for an event that expects the MouseEventArgs parameter.  
    this.button1.MouseClick += this.MultiHandler;  
}
~~~

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

Um auf ein Event zu reagieren, definiert man eine EventHandler-Methode im Ereignisempf�nger. Diese Methode muss der Signatur des Delegates f�r das behandelte Event entsprechen. Um bei Auftreten des Ereignisses Benachrichtigungen zu empfangen, muss die EventHandler-Methode das Ereignis abonnieren.

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