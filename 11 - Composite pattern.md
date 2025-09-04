# Composite pattern
Supponiamo di voler manipolare delle strutture di tipo **composito**: alcuni elementi sono aggregatori di altri elementi. Gli elementi all'interno non sono necessariamente omogenei, tipico per le interfacce grafiche. Tipicamente abbiamo visto librerie per aggregati di tipo omogeneo, mentre per un'interfaccia grafica si ha un contenitore principale, ossia la finestra di applicazione, e molti altri oggetti che non sono necessariamente simili alla finestra: pulsanti, menu e aree di testo sono solo esempi di questi oggetti.\
Non si tratta di definire una classe per definire una finestra di applicazione, ma è qualcosa che può essere creata in mille modi diversi. Lo scopo del **composite pattern** non riguarda la creazione della struttura, ma di facilitarne l'uso: chi usa la struttura non deve far condizionare l'algoritmo alla complessità della struttura, ma bisogna poter **vedere l'intera struttura come se fosse un singolo oggetto**. 

Un modo in cui può essere risolto il problema è di strutturare la classe `Window` come un ADT contenente un array di pulsanti, menu, aree di testo e container di widget:
```java
class Window {
    Buttons[] myButtons;
    Menus[] myMenus;
    TextAreas[] myTextAreas;
    WidgetContainer[] myContainers;

    public void update() {
        if (myButtons != null) 
            for (int k = 0; k < myButtons.length(); k++) myButtons[k].refresh();
        if (myMenus != null) 
            for (int k = 0; k < myMenus.length(); k++) myMenus[k].display();
        if (myTextAreas != null) 
            for (int k = 0; k < myTextAreas.length(); k++) myTextAreas[k].refresh();
        if (myContainers != null) 
            for (int k = 0; k < myContainers.length(); k++) myContainers[k].updateElements();

    }
}
```
Il problema di questo approccio è che l'aggiunta di un array di elementi grafici in più richiede l'aggiunta di un nuovo ciclo `for`. Si eseguono quindi $n$ cicli, uno per tipologia di oggetto. La composizione è limitata a tempo di compilazione. Ci sono quindi tante dipendenze.

Possiamo tuttavia utilizzare il polimorfismo, e definire un'interfaccia usata da `Buttons`, `Menus`, `TextAreas` chiamata `GUIWidget`:
```java
class Window {
    GUIWidgets[] myWidgets
    WidgetContainer[] myContainers;

    public void update() {
        if (myWidgets != null) 
            for (int k = 0; k < myButtons.length(); k++) myWidgets[k].update();
        if (myContainers != null) 
            for (int k = 0; k < myContainers.length(); k++) myContainers[k].updateElements();
    }
}
```
Ancora una volta, tuttavia, distinguiamo il contenitore dall'oggetto semplice. Il mio client quindi distingue un aggregato da un oggetto più semplice.\
Possiamo considerare `WidgetContainer` come una speciale foglia all'interno della nostra gerarchia. D'altronde, `WidgetContainer` è pur sempre un elemento GUI, ma in più ha soltanto la capacità di contenere altri widget. Definiamo allora **un'unica interfaccia `Component`** comune a tutte le classi. Se l'oggetto è semplice, l'operazione del component è implementata. Altrimenti, se la struttura è aggregata questa sarà un aggregato di oggetti che implementano `Component` e che chiama ricorsivamente su tutti i componenti la procedura da implementare. Lo schema del pattern è il seguente:\
![alt text](<../!Images/11 - Composite pattern/image.png>)\
La `Operation()` di `Composite` deve quindi ciclare ricorsivamente per eseguire le `Operation()` degli oggetti di cui è aggregato.\
Una cosa interessante da notare è la **ricorsione object-oriented**: se il client ha accesso alla radice della struttura, l'invocazione di `Operation()` sulla radice della struttura la richiama su tutti gli elementi della struttura. Inoltre, le chiamate possono avvenire in due modi: o dal basso, se prima si ha la chiamata ricorsiva object-oriented e poi l'effettiva operazione per la radice della struttura, o dall'alto, se l'ordine precedente è inverso.

### Esempio: AWT 
Il pattern Composite è ampiamente impiegato all'interno delle librerie AWT, definendo la struttura nel seguente modo:\
![alt text](<../!Images/11 - Composite pattern/image-1.png>)
### Precisazione
Chiaramente se la classe `Composite` implementa una sua `Operation()` e viene estesa, la ridefinizione di `Operation()` richiede una invocazione di `super.Operation()`.\
Nella classe `Composite` inoltre si definisce un aggregato di `Component` che è generico per tutte le sottoclassi. 
### Interfaccia stream 
L'interfaccia stream viene utilizzata per interagire con gli aggregati. Gli stream non hanno necessariamente un aggregato di tipo sequenziale: una `Collection` infatti è iterabile, definendo un ordine implicito, ma nello stream l'implementazione dell'iterazione non è un ciclo. Lo stream quindi può eseguire iterazioni in parallelo ed operazioni sugli elementi. Lo stream definisce tre operazioni:
- `map()`, che fa corrispondere ad un elemento un altro;
- `reduce()`, che riduce l'aggregato di elementi;
- `filter()`, che filtra gli elementi da selezionare.

## Conseguenze del pattern
Un client fa riferimento ad un'unica interfaccia, e può aspettarsi un elemento semplice ma riceverne uno composto. Non si vincola a design time il `Composite` all'utilizzo di alcuni tipi di Leaf ma si possono solo eseguire controlli a runtime. Infine, i tipi di foglie non devono per forza essere parenti tra loro, in quanto si sfruttano legami di composizione, per cui il pattern è **object scope**.\
Bisogna inoltre definire le operazioni di `add()` e `remove()` correttamente: in teoria andrebbero definiti in `Component`. Tuttavia, questo espone il codice ad **errori a run-time**: un `add()` su un `Leaf Component` non ha senso. Pertanto si dovrebbe gestire utilizzando una eccezione. Se invece si evita l'implementazione di `add()` e `remove()` su `Component` ma solo sul `Composite`, si può segnalare un errore in tempo di compilazione, ma questo annulla **trasparenza**.\
Per navigare la struttura gerarchica si potrebbe prevedere nei `Composite` un `getParent()` in modo tale da poter risalire al contrario nella struttura. Se la struttura è complicata, si potrebbe assegnare un ruolo di **proxy** alle leaf. Il `Composite` implementa una **cache** per ridurre quindi le ricerche di valori nei child. Tuttavia, i child devono avere un metodo `getComposite()` per accedere e invalidare la cache del `Composite`.

# Visitor pattern
Supponiamo di avere una struttura composita eterogenea. Supponiamo di voler applicare delle funzionalità alla struttura. Sappiamo che è possibile usare il Composite pattern per creare una struttura composita espandibile e facilmente manipolabile. Le funzionalità di manipolazione erano definite nell'interfaccia `Component`, ma se volessi, ad esempio, avere molte funzionalità nella struttura composita che sono scorrelate tra loro, si ha un **affollamento** negli ADT di funzionalità che sono opzionali. E se volessi aggiungere delle funzionalità in un secondo momento, come si potrebbe fare?

Invece di utilizzare un pattern, potremmo prendere un ADT e sottoclassarlo quando è necessario aggiungere funzionalità. In questo modo il tipo di base viene derivato in più varianti, ognuna delle quali si arricchisce di una funzionalità in più. Ma in questo modo non si riesce a combinare più di due funzionalità assieme in una sola classe: infatti ad esempio in Java non si ha l'**ereditarietà multipla**. Si implementa allora il **pattern Visitor**. Il pattern utilizza la composizione, inserendo le funzionalità aggiuntive in un oggetto `Visitor` che incapsula queste. L'ADT creato è un ADT **degenere**, che incapsula solo funzionalità per operare su altri oggetti. L'idea del meccanismo è di avere un oggetto che attraversa la struttura; il visitor viene passato a ciascun elemento della struttura, il quale invoca la funzionalità del visitor.

Abbiamo tre possibilità per visitare la struttura:
1. ogni elemento sa chi è il suo successore nella visita, come se si trattasse di una lista concatenata. L'elemento quindi riceve il visitor, applica il visitor a se stesso, e successivamente applica il visitor al suo successivo;
2. si utilizza un **Iterator**;
3. il visitor sa come navigare la struttura.

Il caso generale è che la struttura supporta la visita, ma non decide l'ordine della navigazione. Non è neanche il visitor a deciderlo, ma è qualcosa di esterno sia al visitor sia alla struttura, come un iteratore. Affinché il visitor esegua la sua operazione sull'elemento si utilizza un **double dispatch**. Si suppone che ogni elemento abbia la possibilità di accettare un elemento di tipo `Visitor` dall'esterno. A sua volta il `Visitor` ha un'interfaccia con un metodo standard al suo interno `visit()`. L'elemento non conosce la funzionalità `visit()`, e passa alla funzionalità se stesso, in modo tale da dare la possibilità al visitor di accedere all'elemento. Si ha quindi una doppia invocazione: si invoca un metodo sull'oggetto per accettare il visitor, che a sua volta invoca sul visitor un metodo per eseguire la visita. Lo schema del pattern è il seguente:\
![alt text](<../!Images/11 - Composite pattern/image-2.png>)\
Il client dipende da una `ObjectStructure`, il quale è un aggregato (indicato con un pallino, si indica chiaramente con un rombo vuoto) di `Element`, interfaccia che contiene il metodo `accept(visitor)` e che **rimbalza la chiamata a `Visitor`**. Il client fa riferimento ad un `Visitor`, un'interfaccia che supporta le visite ad elementi (in foto `visitElementA(ElementA)`, `visitElementB(ElementB)`). Chiaramente la visita deve supportare le visite ad ogni tipo di oggetto presente nella `ObjectStructure`. Il metodo anonimo `visitElement()` può corrispondere a diverse funzionalità di visita, in virtù della implementazione di `Visitor`. Ogni implementazione concreta di `Visitor` definisce una funzionalità di visita della struttura (somma degli elementi, stampa in HTML, ecc.).\
L'accesso ai dati, inoltre, potrebbe non essere sempre possibile dall'esterno. Per far sì che il visitor possa accedere ai dati esso può essere definito come inner class o può ricevere in input l'oggetto da visitare.

## Esempio
Supponiamo di avere un `Visitor` per un albero binario di ricerca. Allora la visita di un elemento opera nel seguente modo:
```java
Node example = new BinaryTreeNode();
Visitor traveler = new HTMLVisitor();
example.accept(traveler);

// all'interno di accept(traveler)
//...
traveler.visitBinaryTreeNode(this);
```
## Limitazioni
Notare che tale tipo di legame è un tipo di legame **statico**. Il visitor quindi non è espandibile. Pur essendo un pattern basato sugli oggetti, è un pattern che si può applicare solamente a tempo di compilazione. L'interfaccia che viene data ai visitor è strettamente legata ai tipi degli elementi che sono presenti nella struttura.

## Interfacca `Consumer`
Il pattern si ritrova nell'interfaccia funzionale `Consumer`. `Consumer<T>` è un'interfaccia che ha un solo metodo: `void accept(T t)`. I metodi che implementano il metodo `accept()` dell'interfaccia Consumer sono automaticamente manipolabili come **oggetti di prima classe**. Si possono quindi passare metodi a variabili o come valori attuali ai parametri di altri metodi. I metodi che implementano interfacce funzionali diventano oggetti passabili come parametri. Il pattern `Visitor` passa la funzionalità di un oggetto ad un altro oggetto per applicare la visita. Se si può definire un algoritmo come un oggetto, non si ha la necessità di definire un algoritmo per contenerlo.\
Il metodo `forEach()` richiede come parametro un oggetto che implementa l'interfaccia `Consumer<T>`. Ogni metodo o oggetto che implementa `Consumer<T>` può quindi essere passato nel `forEach()`. Lo strumento delle interfacce funzionali permette quindi a un qualsiasi metodo di definire implementazioni di interfacce a runtime sotto la forma di espressioni lambda.

`accept()` non restituisce un valore, per cui o produce una modifica di stato di un oggetto, o produce un **side-effect** (la sua applicazione ha lasciato un residuo), oppure modifica lo stato interno dell'oggetto che implementa `accept()`. Nel caso del visitor, `accept()` può modificare lo stato interno del visitor.

Nelle `Collection` il `Consumer` viene utilizzato assieme ad un Iterator. Con gli stream si ha sempre il metodo `forEach` analizzato precedentemente. Anche negli aggregati iterabili esiste `forEach()`, che non è altro che un metodo `accept()`. 

## Applicabilità del pattern
Generalmente il pattern viene applicato quando una struttura di oggetti ha molte classi con interfacce diverse e si desidera eseguire operazioni dipendenti dalle classi concrete. Si tratta di un pattern ideale anche quando bisogna eseguire operazioni distinte non correlate sugli oggetti in una struttura, evitando così di sovraccaricare le classi con tali operazioni; se le operazioni sono tante, risulta più semplice la gestione del codice. Infine, se le classi della struttura cambiano raramente ma si aggiungono nuove funzionalità: qui non vogliamo variare la struttura, ma le funzionalità da applicare a questa.
Chiaramente se aggiungiamo un nuovo elemento, dobbiamo modificare tutti i `Visitor`.

# Pattern Decorator
Il pattern Decorator è un pattern strutturale che lavora sugli oggetti.\
Supponiamo che un oggetto debba implementare una operazione composta da un numero molteplice e variabile di singole responsabilità. Ad esempio, abbiamo una finestra di testo che può avere opzionalmente una barra di scorriment verticale, una orizzontale, un bordo 3D o un bordo piatto. Abbiamo quindi una serie di accessori alla funzionalità base, ossia il rettangolo che contiene testo, che possiamo di volta in volta mettere o non mettere. Se ponessi tutte queste capacità dentro l'oggetto che rappresenta la finestra di testo, l'oggetto si affolla. Se l'oggetto è inoltre posizionato in alto in una gerarchia di oggetti, anche gli oggetti che si trovano in basso si affollano.

Una soluzione potrebbe essere quella di derivare una sottoclasse per ogni possibile combinazione di opzioni. Ma poiché ci pososno essere molte combinazioni, la soluzione non è accettabile. Utilizziamo allora una interfaccia generale `TextView`, che è dotata di un bordo, una barra di scorrimento orizzontale e una barra di scorrimento verticale. Tuttavia, aggiungere una opzione richiede la modifica di tutte le classi.

La soluzione risiede nel definire l'interfaccia `Component`, implementata da un componente base e da un **`Decorator`**. Nel `Decorator` si ha un riferimento a un componente successivo, che può essere un altro decorator o il componente base. Tutte le tipologie di opzioni e di funzionalità sono delle sottoclassi di `Decorator`. Il pattern permette di **aggiungere dinamicamente funzionalità** ad un oggetto a run-time:\
![alt text](<../!Images/11 - Composite pattern/image-3.png>)\
I decorator quindi aggiungono delle funzionalità in più ad un oggetto base. Si accede alle funzionalità dell'intera struttura attraverso un oggetto di accesso. Quindi nel nostro esempio, il client invocherà la funzionalità sul decoratore relativo al bordo, che invocherà le restanti funzionalità. Si tratta anche in questo caso di **ricorsione object-oriented**, quindi anche in questo caso si può decidere se utilizzare le funzionalità a partire dalla "coda" della ricorsione o dalla testa.

## Applicabilità
Applicare il decorator ci permette di modificare il comportamento dinamicamente. L'oggetto che si crea non è solo un oggetto base, ma una struttura arricchita ad ogni passo. Il decorator funge da **wrapper**, avvolgendo quindi la componente base in un altro oggetto per arricchirne funzionalità.

Lo schema del pattern è il seguente:\
![alt text](<../!Images/11 - Composite pattern/image-4.png>)\
All'interno di `Decorator` è presente un `Component` privato. Solo `ConcreteComponent` non ha un successore ma solo l'operazione. L'operazione di base nei decorator invoca la `operation()` del suo successore.\
Da notare che i `ConcreteDecorator` possono modificare le loro implementazioni di `operation()`, ad esempio creando dei metodi non presenti nel `Decorator` o aggiungendo degli stati.

Una volta aver creato la struttura, essa può essere modificata a **run-time** modificando la catena di istanza. Questo può essere utile in combinazione con il **Prototype pattern**.

Si potrebbe fare un parallelismo con il pattern State. Anche in questo caso si può dire che con il Decorator il client si ritrova un Decorator che cambia tipo in base a quanti Decorator vengono aggiunti. La differenza è che nel Decorator il client vede l'oggetto cambiare tipo, mentre con lo state il cambio di oggetto è un cambio di stato interno. Inoltre, con il decorator vengono aggiunte o sottratte funzionalità, mentre con lo state si **sostituisce interamente** la funzionalità in virtù dello stato. Infine, mentre nel dacorator le istanze vengono aggiunte solo quando è necessario, nello state è necessario avere tutti gli stati per un funzionamento corretto del programma. 