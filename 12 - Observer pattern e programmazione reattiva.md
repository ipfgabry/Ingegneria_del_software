# Observer
L'observer pattern è un pattern comportamentale utilizzato per implementare relazioni tra oggetti.\
Il pattern è utile quando è ricorrente nel nostro software una situazione in cui dei moduli distinti hanno tra loro relazioni di dipendenza **comportamentale** (ad esempio, se un modulo A cambia stato nel modulo B si ha una reazione): ad un certo cambiamento di stato corrisponde l'attivazione di un comportamento. Tale relazione può anche non riguardare solo due moduli, ma in generale $n$ moduli: si può avere in generale un modulo che cambia stato e $n$ moduli che dipendono da questo cambiamento di stato (se il cambiamento di stato occorre, devono automaticamente reagire attivando delle loro funzionalità). La classe il cui cambiamento di stato è rilevante prende il nome di **subject** in quanto è l'elemento osservato, mentre le classi che agiscono in virtù dei cambiamenti del subject prendono il nome di **observer**. Per quanto detto, ogni observer può avere una sua differente reazione allo stesso cambiamento di stato. 

Vogliamo a tempo di esecuzione:
- **modificare il numero di observer a tempo di esecuzione**;
- **modificare il tipo di observer a tempo di esecuzione**.

Vogliamo quindi che la dipendenza dai cambiamenti di stato non richieda dei **legami statici** tra questi oggetti. In maniera ingenua questo pattern potrebbe essere implementato utilizzando dei riferimenti agli observer all'interno della classe subject. Il subject quindi si prende carico di attivare i meccanismi di corrispondenza azione-reazione. Questo approccio tuttavia non permette di cambiare il numero di observer, il tipo di questi e anche il comportamento per ciascun observer, in quanto la reazione è cablata nel metodo. I tipi, inoltre, devono essere conosciuti, quindi non è possibile aggiungere tipi a runtime.\
Come conseguenza dell'applicazione dell'observer pattern, se un subject cambia stato e altri $n$ oggetti reagiscono al cambiamento di stato, si riesce a far sì che ci sia una coerenza di stato tra subject e observer. Se gli observer possono conoscere il cambiamento di stato nel subject, è possibile anche fornire all'observer che tipo di cambiamento di stato è avvenuto. Gli observer possono quindi mantenersi coerenti con quello che è successo nel subject. Questo è tipico di sistemi come database.

L'observer, inoltre, può anche essere observer di più subject. In tal caso è necessario anche capire qual è il subject che ha ricevuto una modifica di stato.

## Model View Controller
Il pattern Observer applicato all'architettura di un intero sistema prende il nome di **Model View Controller**.

Il pattern Model View Controller è un modo di implementare a livello di sistema uno schema architetturale già pensato a priori. Si divide l'applicazione in tre grossi sottosistemi, in cui si separano tre aspetti fondamentali:
- il **controller**, dove si eseguono calcoli e la **business logic** del software;
- il **modello**, la parte che si occupa della **persistenza dei dati** (su quali strutture dati vengono eseguiti i calcoli) e della **rappresentazione dei dati** in memoria;
- la **vista**, ossia la **GUI**, l'interfaccia utente grafica.

Questo schema architetturale non indica solamente i tre moduli base, ma anche come devono interagire tra loro. La struttura fornita dal modello è la seguente:\
![alt text](<../!Images/12 - Observer pattern/image.png>)

Ad esempio, ogni applicazione web utilizza l'architettura fornita dal pattern MVC. Organizzare un'applicazione web con questi tier deriva dall'idea di imporre il pattern strutturale più un controller model col vantaggio che le tre componenti possono essere **remote**. Ad esempio:
- la view è il browser;
- il controller è un'applicazione sul server web;
- il model è il database che conserva i dati del server.

Quando si esegue quindi un'operazione, il controller può protare ad aggiornamenti nella view e nel model. Il sistema opera con un meccanismo di **azione-reazione**. Pertanto l'interazione non avviene tramite **messaggi alle interfacce**, bensì come reazioni a notifiche di eventi. Questo favorisce **disaccoppiamento** all'interno del sistema software.

## Schema del pattern
![alt text](<../!Images/12 - Observer pattern/image-1.png>)\
Il `ConcreteSubject` estende una classe predefinita `Subject`. Ogni `ConcreteSubject` è dotato di un `subjectState` e di due metodi `getState()`, `setState()` per la lettura e la modifica dello stato. La lettura dello stato è necessaria per gli observer, in quanto è necessario capire qual è stato il cambiamento di stato. La classe `Subject` prevede le seguenti operazioni:
- `attach(Observer)`;
- `detach(Observer)`;
- `notify()`.

Il `Subject` pertanto deve avere un **array di `Observer`**. L'interfaccia `Observer` prevede un metodo `update()`. Ogni `ConcreteObserver` è dotato di un `observerState` e implementa la logica di `update()`. Aggiungeremo inoltre in `Subject` una variabile booleana `changed` per ottimizzare il meccanismo di notifica agli observer (come spiegheremo successivamente).

La funzione `notify()` avvisa tutti gli observer conservati nell'aggregato `observers` presente in `Subject`. Ogni observer si aggiornerà in virtù della notifica ricevuta dal subject che invoca `notify()`. 

Subject e observer sono entità **indipendenti tra loro**. Il subject inoltre sa quando i suoi algoritmi modificano in modo significativo lo stato: deciderà quindi di notificare il cambiamento di stato con il metodo `notify()`. La `notify()` si traduce nell'invocazione di `update()` su tutti gli observer.

L'applicazione del pattern snellisce le dipendenze, trasformando la dipendenza tra le classi concrete in una dipendenza tra il subject e l'observer:\
![alt text](<../!Images/12 - Observer pattern/image-2.png>)\
Come si nota, la dipendenza di `ClassA` con le altre classi si è trasformata in una dipendenza tra `Subject` e `Observer`.

### Esempio
Le interfacce grafiche fanno uso di questo pattern. Gli elementi dell'interfaccia grafica possono aggiornare altri observer come reazioni ad altri eventi. 

## Publish/Subscribe, reactive programming
Chiaramente se un metodo utilizza il metodo `notify()` il client deve attendere che ogni observer esegua `update()`. Si tratta di un meccanismo **sincrono**. L'implementazione pertanto rende imprevedibile l'efficienza del programma. Una soluzione è implementare un reale sistema di azione-reazione che gestisca la reattività in maniera del tutto **asincrona**.\
Il framework di azione-reazione esiste come concetto nel paradigma di programmazione generale noto come **programmazione reattiva**. Sistemi come react permettono di scrivere applicazioni in cui il meccanismo di interazione tra componenti si basa sulla reazione di azioni. Standardizzare gli eventi rende possibile la comunicazione.\
Java sposa il framework in una libreria che prende il nome di **Flow**, che fornisce una reattività **asincrona** tra i componenti del programma in maniera del tutto trasparente al programmatore che la utilizza. La libreria fornisce le interfacce `Publisher<T>`, `Subscriber<T>`, `Processor<R, T>` e `Subscription` per astrarre il passaggio di un flusso di oggetti da un `Publisher` a un `Subscriber` **senza che vi siano blocchi per il publisher e senza inondare il subscriber**.

Si utilizza allora una implementazione più efficiente del pattern Observer, ossa la versione **Publish/Subscribe**. Viene introdotto un ADT `Subscription` per disaccoppiare realmente l'esecuzione delle `notify()` dal subject agli observer:\
![alt text](<../!Images/13 - Reactive programming, lambda/image.png>)\
La `BufferedSubscription` che implementa l'interfaccia `Subscription` contiene al suo interno uno spazio dove ospitare le notifiche generate dal sistema. La subscription viene utilizzata da un `SubmissionPublisher` che implementa l'interfaccia `Publisher`. Un `Producer` produce degli oggetti ed esegue dei `submit` alla  `SubmissionPublisher`. In questo modo il `Producer` comunica con un `Publisher` che interagisce con `Subscription`. Ogni `Subscription` invia una notifica al `Consumer`. Lo stesso `Producer` è in grado di comunicare quindi con più `Subscription`, permettendo quindi di decentralizzare queste funzionalità e non inserirle nel `Producer`.

Ogni `Consumer` quindi può avere anche più `Subscription` in base a quanti sono i `Publisher`. Il sistema inoltre permette di gestire in maniera efficiente una relazione molti a molti tra produttori e consumatori. La comunicazione avviene grazie alle implementazioni delle interfacce `Subscriber` da parte del `Consumer` e di `Publisher` da parte di `SubmissionPublisher`

Tra un `Publisher`e un `Subscriber` si può avere un'interfaccia intermedia, ossia `Processor`, che implementa entrambe le interfacce. Il `Processor` è un elemento che serve a poter creare trasformazioni, in quanto si vede un flusso in ingresso e un flusso in uscita. I `Processor` possono essere composti per creare un **flusso di trasformazioni**. Il meccanismo illustrato rappresenta uno **stream** di elaborazioni:\
![alt text](<../!Images/12 - Observer pattern/image-5.png>)

Riassumendo:
- `Publisher<T>` è l'interfaccia che definisce il metodo `subscribe(Subscriber<T>)`, analogo di `attach()` ad un `Observer` nel `Subject`. Si tratta dell'interfaccia per fornire item;
- `Subscriber` è usato per ricevere item, definendo i metodi `onNext()`, `onError()`, `onComplete()` e `onSubscribe()`. Tali metodi corrispondono a `update()` dell'interfaccia `Observer`;
- `SubmissionPublisher` implementa l'interfaccia `Publisher<T>` e invia item ai `Subscriber` **in maniera asincrona** con thread dedicati che chiamano `onNext()` dei `Subscriber` tramite il metodo `submit()`;
- `Subscription` è il collegamento tra `Publisher` e `Subscriber`, permettendo al `Subscriber` di ricevere messaggi o di fermarli. Quando un `SubmissionPublisher` chiama `subscribe`, il `Subscriber` riceve una `Subscription` da cui può ricevere le notifiche con `request(n)`, o annullare la ricezione con `cancel()`.
### Esempio 
Consideriamo il seguente esempio:\
![alt text](<../!Images/12 - Observer pattern/image-3.png>)\
La classe subject utilizza una classe `SubmissionPublisher`. All'observer arriverà come dato dentro la notifica istanze di tipo `Persona`. Lo `Store` implementa un'interfaccia template generica da personalizzare con il tipo `Persona`. `Store` ha una funzione update `onNext(Persona)`, e supporta come si nota nello schema UML quattro tipi di notifiche diverse.

`AddrBook` (subject) non deve derivare da `Subject`, ma ha un **riferimento** a `Subject`: `AddrBook` utilizza `SubmissionPublisher` con il tipo `Persona`. `SubmissionPublisher` utilizza un oggetto `BufferedSubscription`, che gestisce una **coda**. Le notifiche vengono invocate su questo oggetto. Questo oggetto, inoltre, contiene i riferimenti agli oggetti. L'invocazione delle notifiche, inoltre, avviene in un thread separato rispetto al subject. In questo modo gli oggetti non si bloccano tra di loro. 

Le update quindi non sono inserite in un ciclo for, ma sono smistate in una coda gestita da una `Subcription`. Il collegamento con la coda si realizza tramite un riferimento alla coda contenuto negli observer. La ricezione delle notifiche viene gestita con un metodo `receive(n)`, che permette di ricevere un numero di notifiche massimo pari ad $n$. In questo modo la funzione `onNext()` estrae la successiva notifica dalla coda. Si fornisce così:
- asincronia tra subject e observer;
- la possibilità di controllare la velocità di ricezione delle notifiche.

### Considerazioni
`Subject` conosce solo la classe `Observer`. In questo modo:
- i `ConcreteSubject` e i `ConcreteObserver` sono **disaccoppiati** e possono essere riutilizzati e modificati più facilmente;
- gli osservatori possono essere rimossi in qualsiasi momento, in quanto il `ConcreteSubject` non sa quanti sono i `ConcreteObserver`.
 
La notifica inviata da un subject può anche essere ignorata. La notifica, inoltre, può non dire agli Object cos'è cambiato, ma solamente notificare il completamento di un'operazione. In alternativa, il `Subject`: 
- può passare informazioni di stato come parametro di `notify()`. Questa modalità prende il nome di **modalità push**;
- può concedere all'`Observer` l'accesso al suo stato. Questa modalità prende il nome di **modalità pull**.  

Nella versione obsoleta del pattern esiste un flag di stato booleano `changed` in `Subject`. Si prevede un metodo `setChanged()` per impostare il valore di `changed`. Quando si esegue `notify()`, vogliamo far scattare meccanismi in oggetti remoti. `notify()` segnala quindi anche cambiamenti di stato. Ma questi due momenti **non sono sempre coincidenti**. Supponiamo che nel nostro `Subject` ci siano diversi momenti in cui nell'elaborazione dell'algoritmo lo stato può cambiare. Tuttavia, magari basta semplicemente notificare che sia avvenuta almeno una modifica di stato, **ma non ogni modifica**. Non ha senso quindi notificare ogni volta gli observer.\
In generale, vogliamo distinguere momenti in cui è successo un cambiamento significativo per altri oggetti, sebbene questo possa accadere in più punti dell'elaborazione. Invece di invocare `notify()`, si esegue `setChanged()` per impostare la booleana a `true`. Alla fine dell'algoritmo, se `changed` è `true` si esegue `notify()`.

Infine, se si ha una situazione di azione-reazione molti a molti può essere utile l'utilizzo di una **shared look-up table**. Se ogni subject ha tanti observer e viceversa e gli observer sono condivisi, invece di avere i riferimenti conservati tra loro negli observer, basta creare una tabella condivisa, in cui per ogni subject si inserisce la lista di observer condivisi tra loro. In questo modo si evita di duplicare i riferimenti agli observer e non si conservano subject senza observer:\
![alt text](<../!Images/12 - Observer pattern/image-4.png>) 