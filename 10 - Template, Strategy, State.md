# Template method
Sappiamo già che per ridefinire il comportamento di un metodo possiamo utilizzare il polimorfismo, ossia facendo un override del metodo. Tuttavia, questo approccio ha **granularità** relativa al metodo. Vogliamo stavolta però definire l'implementazione di un metodo, ma tralasciare il dettaglio specifico di uno o più passi interni all'implementazione del metodo nella sottoclasse. Definiamo quindi un ADT `AbstractClass` all'interno del quale è definita l'operazione `TemplateMethod()`. Dentro il `TemplateMethod()` sono definite delle operazioni simboliche `PrimitiveOperation()`. Queste operazioni primitive sono **astratte**. Le sottoclassi allora ereditano l'algoritmo ma solo nella sua struttura, ridefinendo le operazioni primitive. 

Il metodo `TemplateMethod()` è un metodo ereditato dalla classe padre, ma richiamando all'interno della classe padre i metodi specificati nelle sottoclassi invertiamo la **struttura di controllo**: la classe padre esegue i metodi della sottoclasse.

![alt text](<../!Images/10 - Template method/image.png>)\
## Template method in Java
In Java il metodo template non dev'essere sostituibile dalle classi. Per far sì che un metodo ereditato non sia sostituibile in override, si dichiara un metodo **`final`**. Viceversa, i metodi che vogliamo che siano sostituibili nelle sottoclassi vengono dichiarati come **`protected`**. \
Ricordiamo che ci sono quattro livelli di protezione in Java: `default`, `private`, `protected`, `public`. Il `protected` aggiunge visibilità del metodo alle sottoclassi anche se non appartengono allo stesso package.
```java
public interface Algorithm {
    void partA();
    void partB();

    public final void algorithm() {
        partA();
        System.out.println("Invariable part");
        partB();
    }
}

public class MyAlg implements Algorithm {
    void partA() { System.out.println("My partA"); }
    void partB() { System.out.println("My partB"); }
}
```
### Esempio: AWT
In Java esistono due gruppi di strumenti per le interfacce grafiche: AWT e la libreria swing. Nella libreria AWT il metodo `paint()` è un'operazione primitiva che è utilizzato nella costruzione del frame. `paint()` dev'essere ridefinito nelle nostre applicazioni.

Nella libreria swing invece sono presenti tre metodi `paint()`, più precisamente definiti in `javax.swing.JComponent` e invocati nel seguente ordine:
```java 
protected void paintComponent(Graphics g);
protected void paintBorder(Grapics g);
protected void paintChildren(Graphics g);
```
I metodi funzionano sempre seguendo la logica del template method e invertendo la struttura di controllo. Ridefiniamo tuttavia i tre metodi:
```java
protected void paintComponent(Graphics g) {
    g.drawString("This is my custom Panel!", 10, 20);
    redSquare.paintSquare(g);
}
```

## Inversione di controllo
Il meccanismo di inversione della struttura di controllo non è legato al template method ma è presente anche in altri pattern.\
Solitamente nel processo di **refactoring** è possibile introdurre dove può essere necessario il template method, individuando un possibile metodo in cui sono presenti tanti punti o blocchi condizionali multipli in cui una certa operazione può essere svolta in più modi. Si segue allora la logica:
1. si scrive l'intero codice in un grande metodo;
2. si divide in passi successivi usando commenti;
3. si incapsula ogni passo in un metodo separato;
4. si riscrive il template invocando i metodi estratti.

Si ripette poi successivamente il passo 1. fino a quando tutti i metodi primitivi hanno la stessa granularità; questi saranno dichiarati come `protected`. Il metodo template verrà poi definito come `final`.

Un caso d'uso del template method riguarda la **lazy initialization**, ossia l'"inizializzazione pigra". Una lazy initialization permette di ritardare l'inizializzazione dell'oggetto nel momento in cui è necessario ottenere l'istanza:
```java
public class Foo {
    private Bar field;
    public Bar getField() {
        if (field == null) field = new Bar(10);
        return field;
    }
}
```
Per modificare il valore di default di inizializzazione di `field`, senza tuttavia **esporlo a qualsiasi valore** come parametro, possiamo applicare il template pattern sul metodo:
```java
public class Foo {
    private Bar field;
    public Bar getField() {
        if (field == null) field = defaultField();
        return field;
    }
    protected Bar defaultField() {
        return new Bar(10);
    }
}
```
In questo modo eventuali sottoclassi di `Foo` possono ridefinire il comportamento di `defaultField()`, cambiando il valore di inizializzazione di `field`.

## Vantaggi e svantaggi del pattern
L'applicazione del pattern permette di implementare le parti invarianti di un algoritmo una sola volta. Le parti uguali dell'algoritmo quindi possono essere scritte una sola volta, ridefinendo poi eventuali parti flessibili in virtù della sottoclasse. Abbiamo così la possibilità di **fattorizzare** la struttura dell'algoritmo, lasciando solo alcune parti interne modificabili. 

# Strategy
Anche in questo caso abbiamo un pattern comportamentale in cui abbiamo un algoritmo che può essere definito in modi differenti. Tuttavia, in questo caso introduciamo un `Context`, in cui è presente `ContextInterface()`. All'interno del `ContextInterface()` è presente un passo che può essere implementato in più modi, individuato con il metodo `AlgorithmInterface()`, presente all'interno di un oggetto `Strategy` esterno. `Strategy` è legato a `Context` con una composizione. Variamo allora una parte interna non perché sottoclassiamo `Context`, ma con una composizione di un oggetto `Strategy`.\
![alt text](<../!Images/10 - Template method/image-1.png>)

Questo pattern non ha quindi un **class scope**, ma un **object scope**, in cui possiamo modificare l'istanza di `Strategy` presente in `Context`, e quindi ridefinire il comportamento in virtù di un oggetto che è intercambiabile. In questo caso abbiamo un algoritmo completo, ma all'interno di questo è presente un passo significativo che ne cambia il funzionamento.

## Esempio: Java Layout Managers
All'interno del package AWT è presente un metodo per la gestione degli oggetti all'interno della finestra, in particolare per l'allineamento degli oggetti grafici. Esistono una serie di costanti statiche definite nella classe `FlowLayout`, che permettono di specificare un valore per indicare la variante di algoritmo da utilizzare:
```java
class FlowExample extends Frame {
    public FlowExample(int width, int height) {
        setTitle("Flow Example");
        setSize(width, height);
        setLayout(new FlowLayout(FlowLayout.LEFT));
        //...
    }
}

``` 
## Esempio 2: algoritmi di ordinamento
All'interno di `Collection` sono presenti algoritmi di ordinamento per l'aggregato. L'algoritmo di ordinamento necessita di un criterio di ordinamento. Possiamo allora dire che il confronto è una strategia da specificare in seguito. In questo caso non possiamo implementare questo con un template, quindi tramite sottoclassi legate a tempo di compilazione, ma utilizziamo la composizione.

### Implementazione in java
Supponiamo di avere una situazione del tipo:
```java
//...
switch(flag) {
    case A: doA(); break;
    case B: doB(); break;
    case C: doC(); break;
    //...
}
```
Invece di scrivere l'istruzione `switch`, possiamo allora definire un metodo `do()`, definendo un metodo astratto `do()` e delegando l'implementazione dell'applicazione della strategia ad una classe esterna. Quindi aggiungiamo **overhead** data la chiamata ad una classe esterna, dovuto al fatto che contesto e strategia sono oggetti composti e distinti.\
Potremmo effettivamente pensare di derivare semplicemente sottoclassi, come nel template method pattern. Tuttavia, questo potrebbe causare una **proliferazione eccessiva di classi**. Riconsideriamo l'esempio del package AWT. La classe `Panel` presenta circa 20 layout, e sono presenti circa 40 sottoclassi di `Component` che possono usarle. Tutte le possibili combinazioni tra i layout e le componenti sono circa 800: si dovrebbero derivare allora 800 classi. Chiaramente questo non è accettabile. 
### Interfaccia `Comparable`, `Comparator`
All'interno di Java il meccanismo strategy è utilizzato per l'interfaccia `Comparable`, che definisce un metodo `compareTo()` per restituire un'informazione su quale elemento è più grande di un altro. L'interfaccia `Comparable` è utilizzata per gli algoritmi di ordinamento, in quanto `compareTo()` viene utilizzato per definire quale oggetto è più grande di un altro. In questo caso notiamo che la `Strategy` è una **interfaccia**. 

Da Java 8 sono state introdotte le **interfacce funzionali**: invece di avere oggetti che sono istanze di classi, possiamo anche istanziare metodi, definendo oggetti che sono in realtà riferimenti non a un'istanza di una classe, ma a un singolo metodo. Un esempio è l'interfaccia **`Comparator`**, diversa dall'interfaccia `Comparable`: la seconda si riferisce ad un attributo di un ADT, la prima invece è un'interfaccia funzionale che definisce un metodo `compare()` con due operandi risolvendo il problema del loro confronto. Le classi che definiscono solo il metodo di confronto, **che non sono legate staticamente ad ADT**, implementano quindi le interfacce funzionali. Inoltre, a differenza di `Comparable`, `Comparator` permette anche di definire confronti che non sono **naturali**, ossia confronti tra elementi di tipi diversi. \
Il meccanismo quindi permette di definire oggetti che non sempre sono l'implementazione in memoria di un ADT, ma semplicemente l'implementazione di un metodo. Possiamo allora definire dei `Comparator` utilizzando **espressioni Lambda**:
```java
Comparator<Player> byRanking = (Player player1, Player player2) -> Integer.compare(player1.getRanking(), player2.getRanking())
```
Questa implementazione è alternativa quindi a `Comparable`. 

`Comparator` è anche dotato di un metodo **statico** `Comparing(mr)`, che prende in input una lambda expression oppure un riferimento ad un metodo e restituisce in output l'implementazione corrispondente di `Comparator`:
```java
Comparator<Player> <byRanking> = Comparator.comparing(Player::getRanking);
```
L'interfaccia `Comparable`, in conclusione, è un'ottima scelta per definire l'ordinamento naturale, quindi il modo principale per confrontare gli elementi. Ci sono diversi motivi per cui utilizziamo anche `Comparator`:
- risulta impossibile di modificare il codice sorgente della classe che implementa l'interfaccia `Comparable`;
- evitiamo di appesantire la nostra classe principale;
- definiamo molteplici strategie di confronto, cosa non possibile utilizzando solo l'interfaccia `Comparable`.

#### Esempio
Supponiamo di avere la classe:
```java
public class Player {
    private int ranking;
    private String name;
    private int age;
    
    // ...
}
```
Vogliamo ordinare un `ArrayList` di `Player`:
```java 
public static void main(String[] args) {
    List<Player> footballTeam = new ArrayList<>();
    Player player1 = new Player(59, "John", 20);
    Player player2 = new Player(67, "Roger", 22);
    Player player3 = new Player(45, "Steven", 24);
    footballTeam.add(player1);
    footballTeam.add(player2);
    footballTeam.add(player3);

    System.out.println("Before Sorting : " + footballTeam);
    Collections.sort(footballTeam);
    System.out.println("After Sorting : " + footballTeam);
}
```
Chiaramente otteniamo l'errore a tempo di compilazione:
```
The method sort(List<T>) in the type Collections is not applicable for the arguments (ArrayList<Player>)
```
Pertanto modifichiamo la classe `Player`, implementando l'interfaccia `Comparable` e definendo come metodo di confronto tra giocatori il ranking:
```java 
public class Player implements Comparable<Player> {
    private int ranking;
    private String name;
    private int age;
    
    //...
    @Override
    public int compareTo(Player otherPlayer) {
        return Integer.compare(getRanking(), otherPlayer.getRanking());
    }

}
```
Adesso se eseguiamo il codice otteniamo:
```
Before Sorting : [John, Roger, Steven]
After Sorting : [Steven, John, Roger]
```
Supponiamo ora di voler ordinare la collezione in base all'età. Allora definiamo il comparatore:
```java
public class PlayerAgeComparator implements Comparator<Player> {
    @Override
    public int compare(Player firstPlayer, Player secondPlayer) {
        return Integer.compare(firstPlayer.getAge(), secondPlayer.getAge());
    }
}
```
Istanziamo un oggetto di tipo `PlayerAgeComparator` e passiamo il comparatore al metodo `sort()`, come se stessimo passando un puntatore a funzione in C++ per il sorting di una struttura:
```java
PlayerAgeComparator playerComparator = new PlayerAgeComparator();
Collections.sort(footballTeam, playerComparator);
```
E adesso vedremo:
```
Before Sorting : [John, Roger, Steven]
After Sorting : [Roger, John, Steven]
```
Alternativamente possiamo definire il `Comparator` nei seguenti modi:
```java
Comparator byAge = (Player firstPlayer, Player secondPlayer) -> Integer.compare(firstPlayer.getAge(), secondPlayer.getAge());
// oppure
Comparator<Player> byAge = Comparator.comparing(Player::getAge);
```
## Applicabilità del pattern
Lo strategy pattern è particolarmente utile quando abbiamo versioni diverse di uno stesso algoritmo, permettendo anche la definizione di versioni future dell'algoritmo. 

Un aspetto importante del pattern riguarda il contesto. `Context` invoca una funzionalità di `Strategy`, che è una classe esterna. `Strategy` tuttavia dovrebbe accedere ad informazioni che conosce il `Context`: ad esempio, nel caso dell'algoritmo di ordinamento `Strategy` deve conoscere gli elementi per confrontarli. Si può allora utilizzare il meccanismo di **inner class**, definendo la classe `Strategy` all'interno della classe `Context`.

Sia nel template sia nello strategy pattern, l'idea è sempre simile: abbiamo un algoritmo e vogliamo ridefinire il comportamento di alcune parti. Il client dei pattern ignorano quale variante è stata utilizzata, ma è il client che ha la possibilità di determinare quale variante utilizzare. Tuttavia:
- nel template pattern la variante è un ADT indicato nel contesto d'uso a tempo di compilazione. L'obiettivo è lasciare un passaggio aperto a varie possibili implementazioni, creando vari ADT;
- nello strategy pattern si definisce una famiglia di oggetti intercambiabili che implementano varianti interne dell'algoritmo, applicandole ad un contesto di alto livello e indicandole a tempo di esecuzione.

## Varianti di implementazione
Possiamo definire in due modi come strategia e contesto interagiscono:
- il contesto può passare i dati alla strategia, come parametro;
- la strategia ha accesso privilegiato al contesto.

Possiamo implementare le strategie da passare come parametri ai contesti, come:
- istanze oggetti, tramite dependency injection se vogliamo cambiarle a runtime;
- usando le generics, a patto che il tipo sia riconosciuto a tempo di compilazione;
- usando le funzioni lambda che implementano i Comparator. In generale, Java Streams è un'applicazione più generale del pattern.

# State pattern
La struttura dello state pattern sembra molto simile al pattern strategy. Tuttavia, qui il focus non è l'algoritmo. `Context` ha un metodo pubblico `request()` ed è dotato di uno **stato**. Per ipotesi, il `Context` è un oggetto con stato, quindi in questo caso ha senso fare un diagramma degli stati. Il metodo `request()` quindi non è sempre lo stesso: `request()` può usare gli attributi dell'ADT `Context`, quindi lo stato del `Context` per decidere come procedere.

`request()` rappresenta un algoritmo complesso il cui comportamento dipende dallo stato del contesto. Possiamo allora inserire ogni ramo condizionale in una classe separata `ConcreteState`, tutte implementazioni di un'interfaccia `State` dotata di un metodo `handle()`. La differenza quindi risiede nel fatto che non abbiamo un oggetto in cui abbiamo un algoritmo che presenta varianti: abbiamo una funzionalità interna, la cui implementazione dipende dallo stato interno che renderebbe complicata l'implementazione. Spostiamo allora la complessità alle classi `ConcreteState`. 

`Context` allora opera come un **automa a stati finiti**. Il Client invoca richieste su `Context`, che a sua volta invoca `handle()` su `ConcreteState` per operare in virtù dello stato di `Context`.\
![alt text](<../!Images/10 - Template method/image-2.png>)

## State vs Strategy
Anche lo state pattern ha **object scope**, ma cambiano frequenza e visibilità dei cambi. Per distinguere state pattern e strategy pattern, utilizziamo queste due informazioni:
- nello strategy applico al contesto **una sola strategia** per ogni ciclo di vita. Nello state, il contesto **muta più volte lo stato concreto** nello stesso ciclo di vita;
- nello strategy pattern tutte le strategie fanno la stessa cosa, ma in maniera diversa: per i client non fa alcuna differenza visibile se cambia il risultato della funzionalità invocata sul contesto. Nello state, gli stati concreti producono **risultati visibilmente differenti nel client**, vedendo il contesto reagire in virtù dello stato.

## Esempio: biglietteria
Supponiamo di avere il seguente sistema:\
![alt text](<../!Images/10 - Template method/image-3.png>)\
Ogni operazione deve controllare in che stato si trova il biglietto, pertanto avremmo del codice del tipo:
```java
public void prenota(String s) {
    switch(stato) {
        case DISP:
            //...
            break;
        case BLOC:
            //...
            break;
            //...
    }
}
```

Una tecnica di refactoring del codice è la **Replace conditional with Polimorphism**, ma in questo caso poiché le azioni dipendono dallo stato del biglietto, facciamo un refactoring utilizzando lo state pattern. Spostiamo la logica degli stati in nuove classi, utilizzando l'interfaccia `StatoBiglietto`. L'implementazione si semplifica in maniera enorme:
```java
public class Biglietto {
    private String codice = "XYZ";
    private int prezzo = 100;

    private StatoBiglietto sb = new Disponibile();

    //...

}

public class Disponibile implements StatoBiglietto {
    @Override public void mostra() {//...
    
    }
    @Override //..

}
```
L'automa non è più concentrato nel contesto, ma **distribuito negli stati**: ogni stato restituisce qual è il prossimo stato da utilizzare. Lo schema UML diventa quindi:\
![alt text](<../!Images/10 - Template method/image-4.png>)


## Implementare le transizioni
Implementare le transizioni tramite lo stato permette di dare molta flessibilità a run-time, rendendo sconosciuto al contesto la conoscenza degli stati. \
Se invece il contesto è responsabile delle transazioni dello stato, gli stati invece sono più semplici, cablando la logica dell'automa nel contesto. Il vantaggio inoltre risiede nella possibilità di leggere la logica dell'automa nel contesto di utilizzo. Inoltre, poiché gli stati sono semplici, questi possono anche essere utilizzati in più automi.

Se gli stati sono senza attributi di istanza, e quindi possono essere condivisi o riusati, allora possiamo utilizzare:
- il pattern singleton per crearli una sola volta;
- il refactoring di un concrete state affinché non si abbiano più variabili di istanza, trasportando le variabili nel contesto.