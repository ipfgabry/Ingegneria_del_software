# Chain of responsibility
Supponiamo di trovarci in una gerarchia di **responsabilità**, ossia di avere un insieme di oggetti che possono gestire collettivamente un insieme di servizi. Questo insieme di servizi viene gestito con granularità differente da ogni oggetto: ai livelli bassi gli oggetti forniscono compiti molto specifici, mentre ai livelli più alti servizi più generici.\
Vogliamo alle spalle di questi oggetti un'altra serie di oggetti che sono in grado di fornire per uno o più oggetti di prima linea la stessa tipologia di servizio ma in modo più generico:\
![alt text](<../!Images/21 - CoR, Command, Mediator/image.png>)

Il client allora richiede un servizio ma non saprà qual è l'oggetto specifico che soddisferà la richiesta: esso farà solo riferimento ad un'interfaccia, ma il meccanismo di catena di responsabilità è del tutto nascosto. Se inoltre il client richiede un servizio ad un oggetto più di basso livello, ma tale oggetto non è adatto a svolgere il compito, allroa la chiamata viene dirottata ad un oggetto di più alto livello che riesce a svolgere il task in una maniera un po' più generale. 

Il pattern nasce dalla scrittura di sistemi di **help contestuale**, ossia messaggi di aiuto per determinate sezioni di interfacce. Se la richiesta di aiuto per un determinato pannello non è soddisfabile, questa richiesta viene dirottata sull'aiuto della finestra generale. Questo è il motivo per cui alcuni messsaggi di help specifici in realtà si rivelano più generici.

La caratteristica peculiare del pattern risiede nel fatto che avendo una catena di responsabilità, il client **non sa chi fornirà il servizio**, de-responsabilizzando l'oggetto a cui è stata fatta la richiesta nel caso in cui esso non sappia gestirla. La struttura della gerarchia è gestita tramite legami di **composizione**, per cui il pattern ha **object scope**. La struttura, inoltre, dev'essere assemblata a **run-time** e non a tempo di compilazione. L'invocazione della richiesta viene gestita tramite una ricorsione object oriented.

Il pattern propposto prende il nome di **catena di responsabilità**, sebbene in realtà si abbia una **gerarchia di responsabilità**.

## Schema del pattern
![alt text](<../!Images/21 - CoR, Command, Mediator/image-1.png>)

I partecipanti alla catena devono estendere la classe astratta `Handler`. La classe astratta prevede un attributo privato `successor` e un metodo `handleRequest()`, che di default richiama il metodo `handleRequest()` su `successor`. Più `ConcreteHandler` definiscono il comportamento nel caso della richiesta.

Chiaramente affinché la catena di funzionalità sia gestita correttamente è necessario comporre nella maniera corretta i successori. Si prevede quindi una fase di impostazione della gerarchia.

## Applicabilità
Il pattern disaccoppia l'invoker della richiesta da chi serve la richiesta. Chi riceve l'invocazione non è quindi chi soddisfa nello specifico la richiesta, ma potrebbe essere uno tra i tanti handler in grado di farlo, anche in maniera non specifica. Si ha quindi un **accoppiamento lasco**.

Possiamo anche immaginare il chain of responsibility pattern come un modo di gestire il **polimorfismo**. Nel polimorfismo un oggetto `Super` può anche essere un oggetto `Derived`, e sebbene il client lavori sull'interfaccia dell'oggetto `Super` in realtà la chiamata a metodo può anche essere dirottata sul metodo dell'oggetto `Derived`. Il polimorfismo prevede una struttura gerarchica **immutabile** e definita **in tempo di compilazione**. Anche nel chain of responsibility abbiamo un polimorfismo. Ma questa struttura non è una struttura di tipi, ma è una struttura dinamica che componiamo durante l'esecuzione del programma, e quindi può essere creata e modificata liberamente. Riusciamo perciò ad ottenere un **polimorfismo fuori gerarchia di classe**, con una logica che può essere variata.

## Chain of Responsibility e Decorator
Il fatto di poter creare la struttura in tempo di esecuzione richiama il pattern Decorator, dove l'estensione di funzionalità viene gestita con una struttura definita a run-time. Tuttavia:
- il set di funzionalità non è prefissato per un certo oggetto, mentre nel chain of responsibility si suppone che **una gerarchia implicita sia preesistente** per la gestione delle richieste;
- entrambe le strutture sono modificabili nel tempo. Tuttavia la catena di responsibilità prevede una catena di oggetti non lineare, mentre il decorator prevede una catena lineare;
- mentre nel chain of responsibility si cerca di **reindirizzare una richiesta** ad un oggetto che possa soddisfarla, nel decorator pattern ogni decoratore **contribuisce alla richiesta**.

## Rappresentare le richieste
Se ci sono molteplici richieste, è possibile implementare queste con metodi con firme precise e diverse dagli altri. Questo tipo di rappresentazione delle richieste prende il nome di **richieste cablate**. Tutte le richieste gestibili però devono essere note fin dal principio. Questo però è limitante, in quanto se bisogna aggiungere una responsabilità, è necessario aggiungere questa ad ogni oggetto della catena di responsabilità.

Invece di codificare le richieste con metodi separati, è possibile implementare un metodo generico `handle()` che riceve in input un parametro che indica la richiesta. Questo parametro può essere un intero o una stringa. Ad esempio:
```java
public void handle(String request) {
    switch(request) {
        case "Open" : //something
        case "Close": //something
        case "New": //something
        default:
            successor.handle(request);
    }
}
```
Questo approccio permette di codificare anche un numero variabile di richieste.

Una soluzione più intelligente risiede nell'**incapsulamento** del metodo che soddisfa la richiesta in un oggetto che viene passato alla funzione `handle()`:
```java
abstract class SingleHandler {
    ...
    public void handle(Request data) {
        successor.handle(data);
    }
}

class ConcreteHandler extends SingleHandler {
    public void handle(Open data) {
        //do something with data
    }
}
```
L'ADT `Request` rappresenta una richiesta generica. Possiamo sottoclassare `Request` per indicare le diverse tipologie di richieste, spostando la gerarchia tra gli oggetti che devono gestire la richiesta nei tipi di richieste. Sono le richieste che devono essere gestite gerarchicamente.\
Chiaramente per capire se il tipo di parametro ricevuto è corretto è necessario ispezionare il tipo di parametro passato utilizzando la Reflection API. Quindi una volta aver ricevuto la chiamata `handle(Request data)`, si ispeziona il tipo di `data` e si può redirezionare la chiamata correttamente.

# Command
L'obiettivo del **command pattern** è incapsulare la richiesta di un'operazione in un oggetto. In questo modo si disaccoppia del tutto l'oggetto che invoca un'operazione da quell oche la esegue. In pratica:
- l'invoker invoca un servizio tramite l'interfaccia di un oggetto;
- l'interfaccia dell'oggetto prevede un metodo `execute()` ;
- l'implementazione dell'interfaccia contiene un oggetto `receiver` a cui viene dirottata la richiesta.

![alt text](<../!Images/21 - CoR, Command, Mediator/image-2.png>)

Se il client configura il `ConcreteCommand` in maniera differente, la stessa invocazione si traduce in un comando differente. Questo ancora una volta permette di sfruttare il mecccanismo del polimorfismo senza gerarchie. Il client è il pezzo dell'applicazione che configura i command perché funzionino. L'invoker può essere un altro elemento di un applicazione che utilizza dei command attraverso l'interfaccia `Command`.\
Un `ConcreteCommand` non è altro quindi che un oggetto che implementa l'interfaccia `Command` e che utilizza funzionalità esterne di un oggetto che incapsula. Il `ConcreteCommand` può:
- semplicemente rimbalzare la chiamata all'oggetto incapsulato;
- partecipa alla creazione della funzionalità. L'oggetto incapsulato in questo caso aiuta nell'esecuzione del comando ma la funzionalità viene fornita dal comando stesso. 

Il pattern è stato identificato facilmente durante la scrittura di **framework** per la gestione di applicazioni. Il command pattern infatti si trova spesso nel momento in cui bisogna implementare strumenti per creare dei menu. 

## Applicabilità
Una situazione simile al command pattern è riscontrata nell'**observer pattern**, in cui il metodo `update()` dell'observer corrisponde a diverse funzionalità in virtù dell'observer coinvolto. Il meccanismo del command pattern, tuttavia, è ancora più sofisticato per il fatto che non definiamo delle funzioni di callback staticamente, ma passiamo comandi a run-time.

Per avere in tempi diversi definizione, invocazione ed esecuzione di comandi l'oggetto command può differire l'esecuzione dell'azione. Il command, inoltre, può anche elaborare dei dati relativi alle richieste ricevute e ispezionare il risultato, in quanto **il command incaspula il receiver**, a differenza di altri pattern come il decorator pattern, dove ogni funzionalità è ignara del risultato delle chiamate di altre funzionalità aggiunte al componente base.

Possiamo anche assemblare funzionalità di alto livello componendo più command assieme. In questo modo funziona il meccanismo delle **macro**. Il meccanismo del command permette anche di eseguire azioni come **undo**, perché nel command siamo in grado di incapsulare il risultato dell'operazione. Quindi basta salvare l'elenco dei comandi con i rispettivi risultati.

Sebben sia un pattern che garantisce molta flessibilità, paghiamo prezzo per:
- la proliferazione delle classi;
- chiamate indirette;
- aumento della complessità. L'indirezione è un bene per disaccoppiare l'invoker dalle specifiche richieste, ma l'**invoker non potrà vedere l'effetto dell'invocazione attivata**. Il pattern ha lo scopo di rendere flessibile l'invocazione, ma l'esagerazione nell'utilizzo dei comandi può rendere incomprensibile la codifica di un algoritmo.
  
## Implementare i comandi
Chiaramente se ci sono $n$ comandi da implementare si ha la necessità di implementare $n$ classi, una per comando. Possiamo fare di meglio utilizzando un'unica classe `Command` che incapsula:
- oggetto ricevente;
- il metodo del comando;
- argomenti da passare.

L'implementazione della classe utilizza la reflection:
```java
public class Command {
    private Object receiver;
    private Method command;
    private Object[] arguments;

    public Command(Object receiver, Method command, Object[] arguments) {
        this.receiver = receiver;
        this.command = command;
        this.arguments = arguments;
    }
    public void execute() {
        command.invoce(receiver, arguments);
    }
}
```

## Command processor
Possiamo gestire i comandi in un unico posto utilizzando un **command processor**. Il command processor si occupa di gestire i comandi di vario genere, istanziandoli ad ogni invocazione. Il command processor dispone di un set di oggetti command pre-configurati e da far corrispondere alle richieste. All'interno del command processor possiamo inserire anche delle istanze di comandi staticamente e applicare il **prototype pattern per clonare i comandi**. Infine, se vogliamo un solo command processor possiamo applicare il singleton pattern.

Il command processor quindi è utile per:
- schedulare l'esecuzione dei comandi;
- fare logging della sequenza d'esecuzione;
- conservare le istanze di tutti i comandi richiesti;
- memorizzare la sequenza di azioni, implementabile tramite una stack;
- implementare un **sistema transazionale**, rendendo una serie di comandi una transazione atomica.

![alt text](<../!Images/21 - CoR, Command, Mediator/image-3.png>)

# Mediator
In un'interfaccia grafica solitamente abbiamo una serie di oggetti che dipendono tra loro. Chiaramente se i componenti dipendono strettamente tra loro, ogni singola modifica su un qualsiasi oggetto impatta gli altri. Vogliamo ridurre queste dipendenze in modo tale da avere un accoppiamento lasco e garantire il riuso.

Possiamo introdurre un **intermediario**, facendo si che invece di esserci un insieme di interdipendenze le dipendenze siano rivolte ad esso. Ogni componente, detto **collega**, interagisce con un mediator, e il mediator dipende dai singoli colleghi:

![alt text](<../!Images/21 - CoR, Command, Mediator/image-4.png>)

Nella sua interfaccia il mediator dovrà incapsulare i metodi e le interfacce esposte dai colleghi. Chiaramente il mediator **non ingloba le funzionalità, ma solo le interfacce**, per cui dirotterà le chiamate. Ad esempio, se una classe `A` chiama un metodo di una classe `B`, allora `A` fa una richiesta al mediator, che dirotta la richiesta a `B`. Il mediator pertanto ha bisogno di rifrerimenti ai colleghi.

Il mediatore concreto necessita di conoscere le classi con cui deve mediare. 
## Differenze con Facade
Il mediator espone un'interfaccia interna e i colleghi riconoscono il fatto che stanno lavorando con un mediator. Non si tratta di una facciata rivolta all'esterno, come accade nel pattern Facade.\
Il pattern Facade, inoltre, non prevede l'aggiunta di funzionalità, mentre il mediator incorpora tutta la logica di funzionamento che prima era distribuita tra le classi, e può anche incorporare nuove funzionalità, proprio per il fatto che il suo ruolo è quello di mediare tra le classi. Il mediator quindi **trasforma un algoritmo decentralizzato**, distribuito tra le classi che richiamavano metodi tra loro, **in un algoritmo centralizzato**.\
Infine, il mediator **istanzia e mantiene i rfierimenti**, coordinando l'interazione tra i colleghi, mentre il facade semplifica l'accesso ai metodi riducendo le dipendenze del client dal sottosistema a cui il facade fa da facciata.

## Varianti
Possiamo far si che i colleghi accedano al mediator:
- implementando nel mediator dei **metodi espliciti** corrispondenti a quelli nei colleghi;
- implementando nel mediator un **metodo unico generico**, passando il colleague per individuare chi lo invoca. Il passaggio del collega mi aiuta quindi a che punto siamo dell'elaborazione;
- implementando nel mediator un **metodo con overloading**, identificando il tipo del collega passato.

# Bridge
Lo scopo del pattern Bridge è quello di **disaccoppiare una gerarchia di astrazioni da una gerarchia di corrispondenti implementazioni**. Normalmente infatti quello che succede è che abbiamo un'astrazione, e l'astrazione può essere implementata in vari modi. Quello che abbiamo visto per ora è che dall'astrazione si derivano più versioni concrete, e quindi più varianti di implementazione. Facendo qussto si blocca la possibilità di espandere l'astrazione, ossia di **raffinarla**.

Supponiamo di avere una forma geometrica, ad esempio `Circle` e `Square`. Supponiamo che sia per `Circle` sia per `Square` vogliamo definire dei colori, ad esempio `Red` e `Blue`. Vogliamo allora implementare i colori per le figure definite. La struttura a cui arriviamo potrebbe essere la seguente:\
![alt text](<../!Images/21 - Ultimi design pattern/image-1.png>)\
Se dovessimo aggiungere un nuovo colore, allora dobbiamo aggiungere altre implementazioni, ad esempio `YellowCircle` e `YellowSquare`. E se aggiungessimo una nuova forma, dobbiamo implementare i colori per questa nuova forma.

Il problema di questa struttura riguarda l'unificazione di astrazione e di implementazione. Vorremmo allora essere liberi di creare una qualsiasi forma e di stabilire il colore senza avere una proliferazione di classi esagerata. Possiamo allora **comporre l'implementazione nell'astrazione fornendo all'implementazione un'interfaccia**:\
![alt text](<../!Images/21 - Ultimi design pattern/image-2.png>)

Il nostro client quindi può fare riferimento alla forma in relazione alle sue funzionalità, e all'interno delle funzionalità possiamo definire un comportamento in base all'implementazione che viene integrata nell'astrazione:\
![alt text](<../!Images/21 - Ultimi design pattern/image-3.png>)

Questo approccio permette anche di definire un'implementazione anche a run-time in maneira dinamica.
## Bridge e Adapter
Il bridge è una scelta che viene applicata a **design time** per far evolvere separatamente astrazioni e implementazioni. L'adapter, invece, affronta il **problema inverso**: connettere **un**'astrazione ed **un**'implementazione, ma senza alterarle.\
Il pattern adapter, inoltre, permette di far funzionare classi che sono completamente scorrelate tra loro. Inoltre, se si devono avere più alternative di implementazione, **si deve cambiare adapter**.\
Il bridge permette di estendere l'astrazione a piacimento, mentre nell'adapter l'astrazione è un **punto fermo**.

# Memento
Ogni oggetto ha un suo stato, però non sempre è necessario che la gestione dei salvataggi dello stato sia una funzionalità che bisogna internalizzare dentro l'oggetto stesso. Lo stato di un oggetto tuttavia dovrebbe essere la parte più privata di un oggetto, non accessibile da nessuno. Quindi da una parte si porta fuori e dall'altra si deve proteggere. 

Incapsulando lo stato ottenuto in un oggetto. Utilizziamo un oggetto **`Memento`** che tiene la copia dello stato:\
![alt text](<../!Images/21 - Ultimi design pattern/image-5.png>)

Un `Originator` istanzia e usa (freccia tratteggiata) il `Memento`, un oggetto con due metodi: `getState()` e `setState()` (non presente in foto). Le funzionalità per caricare e conservare lo stato non sono dentro l'`Originator`, ma dentro il `Memento`.\
Il pattern permette salvataggi incrementali. Se la funzionalità di salvataggio è nel `Memento`, allora esso può avere una copia dello stato precedente e quindi potrebbe anche salvare solo le modifiche allo stato. Ci permette inoltre di non esporre lo stato all'esterno.


Usare i `Memento` può essere costoso se lo stato è complesso. Fare la copia degli stati potrebbe richiedere molto tempo e memoria, per cui potrebbe non essere conveniente. Tuttavia, è un metodo per permettere ad una classe di poter ripristinare uno stato senza esporlo all'esterno.

## Memento e Iterator, Memento e Clone
La navigazione può essere implementata tramite gli iteratori, ma questo è anche possibile con **memento differenti che rappresentano lo stato di differenti iterazioni**. L'incapsulamneto della collezione è sempre preservato, in quanto sapere i link al prossimo elemento è accessibile solo al `Memento`.

Il `Memento` può anche essere implementato con il **prototype pattern**, per il fatto che il `Memento` può essere istanziato come il clone di se stesso. 

# Flyweight
Un **`Flyweight`** è un oggetto leggero che si può avere in un numero di istanze elevatissimo. Abbiamo già visto nel prototype pattern l'esempio di programma per la creazione di note in uno spartito. Le istanze di note erano 2 (nel nostro esempio), ma rappresentare molte note tramite clonazione può essere uno spreco di memoria.

Le note differiscono solamente per la posizione nello spartito. Altre informazioni **che non riguardano una nota precisa** potrebbero essere memorizzate in un'altra struttura dati. A questo punto mi basta tenere solo una piccola pool di istanze di queste oggetti e restituire queste istanze quando servono. In questo modo:
- informazioni che riguardano lo **stato intrinseco di un oggetto** sono memorizzate in una nuova istanza;
- informazioni **di carattere estrinseco** sono memorizzate in una pool predefinita, in modo tale da non ripetere le stesse informazioni in più istanze.

Ad esempio, nel caso dello spartito possiamo definire una nuova nota in virtù della sua posizione, ma salvare alcune informazioni predefinite della nota in un altro spazio, quali:
- rappresentazione del glifo;
- durata della nota.

Il client fa riferimento ad un `Flyweight` astratto, selezionando quello corretto nel pool istanziato. Questo pool è gestito da un `FlyweightManager`. Inoltre, questo può essere utilizzato in coppia con il pattern memento.

Chiaramente il `Flyweight` dev'essere immutabile, perché questo oggetto viene concesso "in prestito". Inoltre, **non possiamo più fare confronti di istanze**, visto che viene sempre data la stessa istanza in prestito.

![alt text](<../!Images/21 - Ultimi design pattern/image-7.png>)

# Proxy
Il proxy non è altro che un agente che opera come sostituto di un'altra entità. 

![alt text](<../!Images/21 - Ultimi design pattern/image-6.png>)
Il client fa riferimento ad un'interfaccia `AbstractSubject`. L'interfaccia è implementata da un `RealSubject` e da un `ProxySubject`. Il `ProxySubject` incapsula al suo interno il `RealSubject` per redirezionare la richiesta. Il proxy è in grado di introdurre un pre-processamento e un post-processamento della richiesta senza che il client se ne renda conto.

Il proxy viene utilizzato per le Java RMI, ossia la **Remote Method Invocation**. Un client e un server immaginano di lavorare con oggetti locali, ma le richieste sono redirezionate ad oggetti su altre macchine. Gli oggetti locali fungono da proxy e redirezionano le chiamate.

Ci sono anche altri motivi per utilizzare il proxy:
- **virtual proxy**, per la lazy inizialization se la creazione di un oggetto è molto costosa;
- **protection proxy**, per fornire diversi livelli di protezione sull'accesso di un oggetto;
- **cache proxy**, per memorizzare i risultati di operazioni costose;
- **firewall proxy**, per proteggere i client locali da accessi esterni;
- **synchronization proxy**, per sincronizzare più accessi allo stesso subject;
- **counting proxy**, per contare i riferimenti agli oggetti. Questo è tipico di sistemi come i garbage collector, che contano i riferimenti agli oggetti: se non ne esistono, si elimina l'oggetto dalla memoria.

# Builder
Il builder pattern consiste nell'incapsulamento di un oggetto il modo in cui si costruisce una struttura complessa. Questo pattern lo abbiamo incontrato già negli stream di Java.

Vogliamo far sì che la struttura creata possa prevedere uno **stesso strumento per la creazione** di questa, ma permettere l'ottenimento di **diverse rappresentazioni**. Un builder quindi si occupa dell'assemblamento della struttura, mentre un client invia solamente la richiesta di costruzione. L'oggetto composto viene creato a run-time, senza una definizione statica: gli oggetti vengono passati di volta in volta.

Il building presenta due fasi:
- acquisizione delle parti;
- fase di assemblaggio.

Il pattern permette:
- di separare l'implementazione della struttura dall'assemblaggio di questa;
- di isolare il codice di costruzione e rappresentazione.

![alt text](<../!Images/21 - Ultimi design pattern/image.png>)
