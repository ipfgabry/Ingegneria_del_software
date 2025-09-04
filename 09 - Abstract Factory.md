# Abstract Factory
Supponiamo di sviluppare un'applicazione grafica per realizzare interfacce. Vogliamo una flessibilità per cambiare l'interfaccia grafica, ma anche far sì che cambiare da una modalità di interfaccia ad un'altra modifichi l'intera interfaccia.\
Vogliamo quindi che sia rispettato un **vincolo**: cambiando versione dell'interfaccia, modifichiamo le versioni degli oggetti in blocco.

In questo caso abbiamo bisogno di un `Creator` che permetta la produzione di oggetti correlati; la factory dev'essere un **oggetto separato** dal client. 

Ad esempio, supponiamo di avere un menu da dover riempire con determinate opzioni, come segue:
```java
public void installDisneyMenu() {
    Menu disney = //crea un menu...
    disney.add("Disney world");
    disney.add("Donald duck");
    disney.addGrayBar();
    //...
}
```
Se volessimo modificare il menu è necessario modificare il codice, diventando meno flessibile. La soluzione è passare al client un oggetto `Factory`:
```java
public void installDisneyMenu(WidgetFactory myFactory) {
    Menu disney = myFactory.createMenu();
    disney.addItem("Disney World");
    //...
}
```
Nel nostro codice `WidgetFactory` è una classe **astratta**. La abstract factory permette di definire una serie di metodi astratti che verranno implementati per la produzione di un set di oggetti.\
![Schema Abstract Factory](<../!Images/09 - Abstract Factory/image.png>)
## Varianti di implementazione
Come nel factory method, abbiamo molteplici varianti di implementazione del pattern.
### Variante classica
Si definisce una classe astratta `AbstractFactory` e si definiscono più classi derivate per le singole factory:
```java
abstract class WidgetFactory {
    public Window createWindow();
    public Menu createMenu();
    public Button createButton();
}

class MacWidgetFactory extends WidgetFactory {
    public Window createWindow() { return new MacWindow(); }
    public Menu createMenu() { return new MacMenu(); }
    public Button createButton() { return new MacButton(); }

}
//...
```
In questo modo il client si libera dalle dipendenze dirette ma fa riferimento ad una abstract factory. Il legame della abstract factory con le sue sottoclassi è un legame di tipo statico.
### Variante con composizione
Si evita un legame statico, utilizzando al posto della derivazione la **composizione**. Si utilizzano riferimenti a factory di prodotti **esterne**.\
In questo caso nella classe astratta definiamo il comportamento dei metodi, utilizzando delle classi pre-esistenti:
```java
abstract class WidgetFactory {
    WindowFactory wf;
    MenuFactory mf;
    ButtonFactory bf;

    public Window createWindow() { return (Window) wf.createFancyWindow(); }
    public Menu createMenu() { return (Menu) mf.createMenu(); }
    public Button createButton() { return (Button) bf.createMenu(); }
}

class MacWidgetFactory extends WidgetFactory {
    public MacWidgetFactory() {
        wf = new MacWindowFactory();
        mf = new MacMenuFactory();
        bf = new MacButtonFactory();
    }

}
```
Affinché funzioni basta collegare il codice con l'implementazione di `MacWindowFactory`, `MacMenuFactory` e `MacButtonFactory`, che si suppone siano implementazioni pre-esistenti e che implementano le funzioni di creazione di finestre, menu e pulsanti. Deleghiamo quindi alle sottoclassi l'inizializzazione delle factory da usare. 

### Variante con Reflection API
Basarsi sulla Reflection API permette di far riferimento ai nomi delle classi dei prodotti che vogliamo utilizzare nel momento di creazione delle factory da utilizzare. Si utilizza quindi un solo creator per tipo di oggetto e si parametrizza per la versione di oggetto.

È possibile modificare il codice con la seguente logica: invece di creare dei creator per il prodotto, utilizziamo i creator semplicemente per dire qual è la classe di prodotto da utilizzare:
```java
public abstract class AbsFact {
    abstract Class<? extends Window> windowClass();
    abstract Class<? extends Menu> menuClass();
    abstract Class<? extends Button> buttonClass();

    public Button getButton() { return (Button) buttonClass().getCostructor().newInstance(); }
    ...    
}

public class MacWidgetFactory extends AbsFact {
    Class<? extends Window> windowClass() { return MacWindow.class; }
    ...
}
```

### Variante su prototipi
Esiste un'ulteriore versione basata sui **prototipi**. Analizzeremo questa variante quando parleremo del prototype pattern.

## Vantaggi del pattern
Il motivo per cui utilizziamo il pattern risiede nella volontà di avere indipendenza dal tipo concreto di prodotti creati e usati. Vogliamo poter configurare il sistema con una tra famiglie di prodotti, imponendo agli utenti dei prodotti di usare **set di prodotti** e non singoli prodotti. 

Lo svantaggio nell'utilizzo di tale approccio risiede nel fatto che in fase di design è necessario identificare correttamente il set di prodotti: una volta che esso è definito, **non è più possibile cambiarlo**. Non si possono creare nuovi set che abbiamo nuovi oggetti rispetto agli altri.

### Combinazione con altri pattern
L'abstract factory pattern viene solitamente utilizzato:
- con il pattern Singleton per garantire che vi sia un'unica istanza di factory;
- con il pattern Prototype, per implementare la factory utilizzando i prototipi.

# Prototype Pattern
La domanda che ci poniamo è la seguente:
> *Come possiamo far funzionare un programma a run-time senza che a tempo di compilazione si conosca l'ADT dell'oggetto?*.

L'idea è la seguente: usare un'istanza di un oggetto originale come **stampo** per crearne altre. Si duplica così un'istanza base:
```
Product obj = prototype.clone()
```

Un **prototipo** deve avere un metodo `clone()` in modo tale che esso sia in grado di duplicarsi. Una volta duplicato il prototipo, le istanze che sono cloni possono essere configurate nei loro attributi per essere differenti l'una dall'altra.

![alt text](<../!Images/09 - Abstract Factory/image-4.png>)

## Uso del pattern
Come negli altri pattern creazionali, vogliamo che il sistema sia indipendente dal prodotto concreto che utilizza, ma in più si ha la conoscenza della classe concreta **solo a runtime**, quindi è sconosciuta a tempo di design. Stabilire tale classe prima infatti stabilisce una dipendenza. In pratica, l'**oggetto a run-time funge da classe, affinché il client non dipenda da quella classe**. Questo è utile in scenari in cui, ad esempio, il client conosce l'interfaccia dell'oggetto ma non conosce l'implementazione della classe; per conoscere quest'ultima, è necessario legare il client alla definizione della classe, creando una dipendenza. \
Un altro vantaggio riguarda classi che hanno pochi stati possibili. Poiché abbiamo poche versioni di istanze dello stesso ADT, invece di definire più ADT, basta creare dei prototipi da clonare.\
La clonazione può anche essere un processo **meno costoso**. Supponiamo ad esempio di avere la seguente situazione:\
![alt text](<../!Images/09 - Abstract Factory/image-1.png>)\
Se dovessimo disegnare delle note, ad esempio `WholeNote` o `HalfNote`, cambia solamente il glifo, ma non abbiamo bisogno di istanziare un oggetto per ogni glifo disegnato. Bastano solo due istanze da clonare quante volte voglio.\
Vedere gli oggetti come prototipi permette che un oggetto configurato in modo diverso possa essere visto come una **classe**: si hanno prototipi dello stesso ADT ma diversi da loro in virtù del loro stato (i.e. configurazioni limitate degli attributi). Andiamo quindi oltre il concetto di ADT:
- l'ADT è un tipo astratto definito a tempo di compilazione;
- il prototipo è un "ADT" costruito a runtime, che non è prevedibile a tempo di compilazione e che non vogliamo sia specificato nella definizione software.

Il metodo `clone()` invocato su un oggetto può essere:
- una clonazione snella (**shallow clone**);
- una clonazione profonda (**deep clone**). L'implementazione di `clone()` in questo caso deve ricorsivamente clonare gli oggetti a cui è legato.

## Clone in Java
In Java è implementata l'interfaccia `Cloneable`, interfaccia vuota che include il metodo `clone()`. Il metodo `clone()` crea una shallow copy, copiando valore per valore le variabili di istanza. Se i valori sono riferimenti ad altri oggetti, saranno allora gli stessi riferimenti di prima, quindi gli oggetti composti saranno condivisi. Per implementare una deep copy, bisogna eseguire un override del metodo `clone()`. Per poter invocare il metodo `clone()`, anche quello di default ereditato da `Object`, non è possibile invocarlo se prima non viene specificato che l'oggetto funzioni con il prototipo: la classe deve quindi implementare l'interfaccia `Cloneable`. Se non viene implementata l'interfaccia, si crea un'eccezione `CloneNotSupportedException`. Nel momento in cui utilizziamo il metodo `clone()` il nostro metodo deve avere la capacità di fare un raise dell'eccezione, quindi dobbiamo aggiungere `throws CloneNotSupportedException`. 

## Conseguenze del pattern
Per ipotesi gli oggetti con cui lavoriamo sono definiti a run-time. È più semplice quindi modificare gli oggetti senza modificare il client. Inoltre, è anche un modo per mantenere celata una struttura complessa all'interno. 

Chiaramente il prototype pattern richiede l'implementazione del metodo `clone()`.
## Prototype manager
Può essere comodo memorizzare i prototipi a cui i cloni fanno riferimento in una struttura, utilizzando ad esempio un simbolo per identificarli. In combinazione con il pattern prototype viene utilizzato il **prototype manger**.\
Generalmente il manager viene implementato con una hash table, e con una funzione che preso in input una chiave restituisce il prototipo associato:
```
prototypeManager.get(key).clone()
```
## Pattern prototype per la creazione di una abstract factory
```java
class ProtoFactory {
    WindowPrototype window;
    MenuPrototype menu;
    ButtonPrototype button;

    public ProtoFactory(WindowPrototype wp, MenuPrototype mp, ButtonPrototye bp) {
        this.window = wp;
        this.menu = mp;
        this.button = bp;
    }

    public Window createWindow() { return (Window) window.clone(); }
    public Menu createMenu() { return (Menu) menu.clone(); }
    public Button createButton() { return (Button) button.clone(); }
}
```

# Adapter Pattern
L'**adapter pattern** viene utilizzato per convertire l'interfaccia di una classe in un'altra interfaccia che i client attendono. \
L'adapter esiste in due versioni:
- il **class adapter**, che consente l'override di parte dei metodi dell'adapter;
- l'object adapter, che adegua l'interfaccia di più classi a quella del client senza sottoclassare. Tuttavia, l'object adapter non sa quale sottoclasse specifica di Adaptee sta usando.

![alt text](<../!Images/09 - Abstract Factory/image-2.png>)\
In foto:
- `Target` rappresenta l'interfaccia che il client si aspetta;
- `Adaptee` è un oggetto che necessita l'adattamento;
- `Adapter` converte la chiamata del client all'interfaccia della classe `Adaptee`. 

Nella versione class adapter, `Adapter` è sia `Target` sia `Adaptee`. Nella versione object adapter, `Adapter` **compone** `Adaptee` dentro sé stessa. 

Da notare che nel momento in cui abbiamo anche più versioni di `Adaptee` derivate da precedenti (quindi altri `Adaptee` che derivano l'originaria `Adaptee`), `Adapter` funge da convertitore anche per l'interfaccia di queste sottoclassi.

`Adapter` è una composizione di `Adaptee` in quanto `Adapter` esiste in virtù di `Adaptee`.
## Funzionalità dell'Adapter
L'adapter funge da sostituto dell'oggetto che implementava `Target`, ma **non aggiunge funzionalità**. L'adapter fornisce funzionalità per un oggetto con un'interfaccia diversa, e teoricamente non deve aggiungere nuove funzionalità. Può tuttavia modificare il metodo chiamato occultandolo al client.

# Facade Pattern
Supponiamo di avere diversi client che dipendono da uno stesso insieme di classi. Il numero di dipendenze può essere elevato; vogliamo che tale numero di dipendenze sia ridotto.\
Possiamo dire che tale insieme di classi crea un **sottosistema** (e possibilmente appartengono ad uno stesso package). Possiamo allora inserire un'ulteriore classe, detta **classe facciata** (**Facade class**) che funge da interfaccia semplificata per i client. In questo modo i client non dipendono direttamente dall'insieme dei classi, ma semplicemente utilizzano la facade per eseguire le chiamate delle classi.

Il ruolo della classe facade non è banale: il facade non deve semplicemente raccogliere le interfacce, ma anche istanziare gli oggetti per il client. È quindi un **aggregatore** di classi del sottosistema.\
Da notare che comunque il client è in grado di conoscere se esiste una classe nel sottosistema. L'unica cosa che abbiamo fatto è aggiungere un'interfaccia in modo tale da non avere una dipendenza specifica con le classi mascherate dalla facade. 

## Funzionalità del facade
La classe facade **non deve aggiungere funzionalità** non presenti già nel sottosistema, in quanto è concettualmente sbagliato. Il facade non dev'essere un modulo software con una propria identità, ma deve solamente **semplificare l'accesso alle funzionalità delle classi che maschera**:
- può controllare condizioni per l'invocazione di una funzionalità;
- può verificare se il risultato di una certa elaborazione soddisfi requisiti;
- verificare che il client che invoca una funzione abbia diritto ad accedere ad alcuni metodi.

Il facade quindi può quindi rielaborare e comporre funzionalità, ma solo a fini di semplificare l'esecuzione e/o l'accesso ai metodi delle classi. 

La classe facade, tuttavia, **non occulta la presenza delle classi che maschera**. Inoltre, ha la responsabilità, come anticipato precedentemente, di istanziare gli oggetti al posto del client. Ciò che fa il pattern è ridurre il numero di dipendenze all'interno del software.\
![alt text](<../!Images/09 - Abstract Factory/image-3.png>)  

Il collegamento marcato con $???$ esplicita il fatto che, sebbene sia concettualmente **insensato**, è ancora possibile realizzare dipendenze di questo tipo. 
