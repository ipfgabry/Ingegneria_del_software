# Modeling e Design
La prima attività che viene eseguita nella progettazione del modulo software è l'**analisi dei requisiti**. L'attività di analisi dei requisiti è la più critica, in quanto individuiamo quali sono i pezzi del nostro sistema e come sono relazionati tra di loro.\
Nell'analisi dei requisiti del sistema realizziamo tre tipi di **modellazione**:
- una **modellazione funzionale**, che consiste nell'analisi del sistema a livello di cosa deve fare il sistema. Il risultato di questa modellazione è un insieme di diagrammi use case;
- una **modellazione a oggetti**, che consiste nell'analisi del sistema a livello di struttura, individuando le entità del sistema, i loro tipi e come queste siano connesse tra loro. Tale modellazione produce diagrammi di classe;
- una **modellazione dinamica**, che consiste nell'analisi del sistema a livello di reattività del sistema a eventi esterni, e come gli eventi vengano gestiti nel sistema. Tale modellazione porta alla produzione di diagrammi di sequenza, di attività e di stato.

Raccolte le informazioni funzionali, sugli oggetti e dinamiche, abbiamo ancora un'analisi formale del sistema: abbiamo infatti solo una collezione di informazioni. Lo step successivo è quello di definire com'è strutturato il sistema. Definiamo quindi uno schema complessivo del sistema, ossia un'**architettura** del sistema. Utilizziamo la **modularità** per creare un'organizzazione del sistema in parti relazionate tra loro. Questo scheletro non è solamente uno scheletro e basta, ma abbiamo anche definito il perché sono stati assemblati i pezzi del sistema in un determinato modo.\
Per rendere funzionante il sistema, risulta necessario spesso anche aggiungere degli oggetti dovuti ad una successiva **object design**, ossia classi che hanno senso in virtù dell'implementazione che andremo a scrivere. Nella fase di object design applichiamo i design pattern visti.\
Ottenuti i documenti che descrivono la relazione tra gli oggetti, i metodi e gli attributi di questi, siamo in grado di passare alla fase di **implementazione** di questi oggetti. Successiva all'implementazione, infine, è la fase di **testing**, utilizzando gli strumenti che abbiamo visto precedentemente.

![alt text](<../!Images/17 - Modeling e design/image-3.png>)

## Analisi dei requisiti
L'analisi dei requisiti prevede due step:
- **requirements elicitation**: si utilizza un linguaggio del dominio, ossia il vocabolario utilizzato è un vocabolario che viene compreso dal cliente. Il sistema viene definito in un linguaggio non tecnico e porta alla produzione del **documento dei requisiti**. Si tratta di una fase che richiede collaborazione tra gli utenti, che hanno una conoscenza relativa al dominio applicativo, e gli sviluppatori, che invece hanno una conoscenza relativa alla soluzione da sviluppare. 
- **requirements analysis**: si utilizza un linguaggio prettamente tecnico, definendo le specifiche tecniche del sistema con un vocabolario compreso dallo sviluppatore, concependo quindi le **specificazioni del sistema**, ossia un modello dei requisiti.

Possiamo distinguere due tipi di requisiti:
- i requisiti **funzionali**, che sono dei requisiti che descrivono cosa deve fare il sistema, specificando le funzionalità del sistema da sviluppare, il comportamento e le azioni da intraprendere all'interno di quest'ultimo;
- i requisiti **non funzionali**, che sono dei constraint di qualità che il sistema deve soddisfare in virtù del progetto da sviluppare. In generale sono dei requisiti che riguardano come una certa elaborazione deve avvenire, e possono includere dei requisiti tecnici.

Oltre ai requisiti vogliamo anche avere una pianificazione del sistema da sviluppare, specialmente a livello temporale. Identifichiamo delle **milestone**, ossia dei punti del tempo in cui si prevede il raggiungimento di certi obiettivi (i.e. il completamento di una certa attività o di un punto di sviluppo dell'implementazione).\
Una specifica importante è l'**ambiente di deployment del sistema**, ossia quello in cui opera il sistema.\
Infine, è necessario anche tener conto dei **criteri di accettazione del cliente**, utilizzati per testare il sistema e vedere se i requisiti sono soddisfatti.

All'interno del documento dei requisiti non sono presenti:
- l'**architettura del sistema**, che rappresenta in realtà la prima risposta ai requisiti funzionali e non;
- la **metodologia di sviluppo**, ossia il tipo di tecnologie che si vanno a utilizzare;
- l'**ambiente di sviluppo**, in termini non solo di macchine utilizzate, ma anche di risorse umane e di competenze necessarie;
- il **linguaggio di implementazione**;
- la **riusabilità**, una qualità su cui bisogna ragionare a livello implementativo.

## Modellazione degli oggetti
Per modellare gli oggetti, consideriamo uno use case e analizziamo il flusso di eventi. In particolare, procediamo con un'**analisi logica** dello use case, individuando:
- **nomi ricorrenti**, che possono determinare categorie di elementi di un dominio, ossia i **tipi** di istanze degli oggetti. Tra questi nomi possiamo individuare dei **nomi propri** che possono identificare delle entità precise, rappresentate da oggetti nel sistema;
- **verbi**, che fanno riferimento ad azioni svolte, quindi possono essere richieste di servizio o produzione di dati. Pertanto identificano **procedure** di cui il sistema deve tenere traccia.

Ad esempio, supponiamo di considerare il seguente flusso di eventi. Il cliente entra nel negozio per comprare un giocattolo per un bambino di età $n$. Il cliente può ricevere assistenza in meno di un minuto, e il proprietario del negozio può dare consigli al cliente in base all'età del bambino. Se il cliente sceglie un giocattolo che può essere pericoloso per un bambino di una certa età, il proprietario deve consigliare un altro prodotto. Possiamo procedere all'analisi testuale dei requisiti. Ad esempio:
- il nome comune "giocattolo" identifica la classe giocattolo;
- il verbo "comprare" indica un metodo;
- il verbo "deve" identifica un constraint;

Generalmente, inoltre, un verbo transitivo identifica un metodo, un verbo intransitivo un evento.

Quando modelliamo un oggetto, tuttavia, bisogna stare attenti alle funzionalità utilizzate dall'oggetto. Ad esempio, supponiamo che a partire dall'analisi logica dei requisiti abbiamo ottenuto una classe `BriefCase`, che modella una valigia. Supponiamo che nei requisiti sia specificata la possibilità di sedersi sull'oggetto che abbiamo modellato come una valigia. La domanda che ora ci poniamo è la seguente: **abbiamo correttamente modellato l'oggetto?**\
La domanda sorge per il fatto che se effettivamente l'azione di sedersi è un'operazione ricorrente, e la valigia non viene mai aperta o chiusa, non è realmente una valigia, ma una sedia. Astraendo l'esempio, **abbiamo modellato in maniera errata l'oggetto**, in quanto non si tratta di una valigia. Per identificare correttamente gli oggetti allora è necessario far riferimento all'uso di questi, ossia alle loro **interfacce**: se partiamo dalla descrizione non funzionale, ossia dagli attributi, la modellazione potrebbe essere fuorviante. La modellazione funzionale dei requisit pertanto è fondamentale per validare la modellazione ad oggetti.

## Modellazione dinamica
Modellati correttamente gli oggetti, possiamo passare alla **modellazione dinamica**, definendo un modello dinamico del sistema tramite un insieme di diagrammi di stato. Più precisamente, si utilizza un diagramma di stato per ogni classe con importanti proprietà dinamiche. L'obiettivo di questa modellazione è modellare le interazioni tra gli oggetti del sistema e i comportamenti dinamici di ogni singolo oggetto. Utilizziamo allora:
- i diagrammi di attività per descrivere gli scenari;
- i diagrammi di interazione / sequenza per modellare le interazioni tra gli oggetti;
- i diagrammi di stato per modellare i comportamenti dinamici di ogni singolo oggetto.

Si presuppone di aver eseguito una **decomposizione** degli elementi del sistema e delle funzionalità di questo. Il primo lavoro con l'analisi dei requisiti è la decomposizione.\
In primis effettuiamo una **decomposizione funzionale**. Il goal principale viene diviso in funzioni più semplici. Ad ogni funzione corrisponde un **livello di astrazione**; inoltre, ogni funzione ha sempre la stessa struttura algoritmica:
- ottieni l'input;
- trasforma l'input;
- produci un output.

Quando scomponiamo le funzionalità, bisogna fermarsi ad un livello di granularità appropriato. Questa granularità corrisponde a quella degli **oggetti del dominio** già identificati. Quindi la decomposizione funzionale deve procedere fino a quando non si incontra la modellazione ad oggetti vista nello step precedente, ossia quando le funzionalità utente sono ad un livello di astrazione corrispondente a responsabilità associabili agli oggetti del sistema. 

Decomporre solo funzionalmente, inoltre, non è sufficiente per la modellazione corretta del sistema. Risulta essere necessario effettuare una **decomposizione object-oriented**. Questa decomposizione focalizza l'attenzione sugli agenti che causano le operazioni. Solo in questo modo possiamo raggiungere il livello corretto di granularità.\
In questo modo una volta identificate le classi e le funzionalità correttamente, possiamo mappare le funzionalità una per una dentro le classi più appropriate. Questo perché sebbene abbiamo individuato le funzionalità presenti nel sistema in gruppi, non è detto che le categorie di entità presneti nel sistema coincidano con i gruppi di funzionalità.

Complessivamente abbiamo un elenco di funzionalità. Formuliamo uno scenario per ogni funzionalità, identificando use cases del sistema. Identificati gli use cases analizziamo il flusso di eventi, e generiamo dei diagrammi di classi, con i seguenti step:
1. identificazione delle classi tramite analisi testuale;
2. identificazione degli attributi e delle operazioni;
3. identificazioni delle associazioni tra le classi;
4. identificazioni delle molteplicità;
5. identificazione dei ruoli;
6. identificazione dei constraint.

## Relazioni nei modelli
Gli strumenti di astrazione e decomposizione ci portano a insiemi di classi. Vogliamo gestire la complessità del sistema mettendo in relazione tra loro queste classi. Tra le più importanti vi è la relazione di **gerarchia**, tra cui:
- gerarchia **part-of**, rappresentabile con diagrammi UML e la notazione composizione / aggregazione;
- gerarchia **is-kind-of**, ossia diagrammi di classe con solo relazioni **is-a**;
- gerarchie **uses**, ossia diagrammi di classe con solo dipendenze d'uso.

## Design del sistema
Realizzare il design architetturale del sistema significa ragionare su come organizzare, raggruppare e assemblare i moduli del sistema, e come questi moduli devono essere relazionati tra loro. La fase del design di un sistema produce l'**architettura di un sistema**, che è la definizione del sistema in termini di componenti computazionali e di interazione tra questi.\
Nella fase di design, dobbiamo capire:
- come selezionare i moduli;
- come definire le interfacce dei moduli;
- come definire le relazioni di uso dei moduli.

Un modulo dev'essere un'unità autonoma, indipendente. Dobbiamo applicare lo stesso principio che abbiamo applicato alle classi: **massima coesione e minimo accoppiamento**. Quello che vogliamo inoltre è che il modulo funzioni come "classe": quello che inseriamo in un modulo dev'essere protetto dall'esterno, secondo il principio di information hiding. Questo è il **golden principle** definito da Parnas: definiamo cosa vogliamo nascondere e definiamo un modulo intorno a questo. Infine, definiamo i moduli in modo tale che abbiamo un'interfaccia minimale.\
La granularità dei moduli è importante, per cui si seguono alcuni principi.

### Reuse/Release Equivalence Principle (REP)
La granularità del riuso è la granularità del rilascio. Il riuso richiede un tracciamento dei rilasci da un sistema di controllo dei cambiamenti.

Questo significa che il codice non dev'essere semplicemente utilizzato copiandolo e incollandolo nelle nostre classi, ma dev'essere riutilizzato **includendo il modulo**. Inoltre, non posso riutilizzare nulla che non venga rilasciato: tutto quello che dev'essere riutilizzato dev'essere anche rilasciato. Ad esempio, utilizzando le Collections di Java stiamo riusando questo modulo, il cui rilascio viene tracciato utilizzando il versioning. Quindi la granularità del riuso del modulo è uguale alla granularità del suo rilascio.

### Common Reuse Principle (CRP)
Le classi in un package sono riutilizzate insieme. Se si riutilizza una classe nel package, si riutilizzano tutte le classi. Ad esempio, una classe che implementa una collection e l'iteratore corrispondente alla collection vengono inseriti nello stesso package.

### Common Closure Principle (CCP)
Le classi in un package devono essere chiuse insieme quando vengono effettuati cambiamento delllo stesso tipo. Un cambiamento che influenza un package influenza tutte le classi nel package.

Essenzialmente si raggruppano le classi che cambiano per lo stesso motivo in un unico package, in modo tale che un cambiamento nei requisiti influenza solamente un numero minimo di classi.

### Acyclic Dependencies Principle (ADP)
La struttura delle dipendenze tra i pacchetti dev'essere un grafo aciclico, ossia non ci devono essere cicli nelle dipendenze.

Questo garantisce che per testare un solo modulo del sistema non devo testare tutto il sistema. Un grafo ciclico delle dipendenze quindi indica che quando bisogna eseguire dei test di integrazione è necessario testare l'intero sistema, rendendo impossibile quindi i test di sottoparti del sistema.

Per rompere il ciclo, basta applicare il **Dependency Inversion Principle**: si crea un nuovo package da cui dipendono i moduli che individuano un ciclo e si inserisce in questo package le classi da cui dipendono questi moduli. Ad esempio:
![alt text](<../!Images/17 - Modeling e design/image.png>)\

La dipendenza da `MyDialogs` a `MyApplication` può essere invertita facendo dipendere `MyDialogs` a una classe interna a `MyDialogs`, che è sottoclasse della classe presente in `MyApplication` come segue:\
![alt text](<../!Images/17 - Modeling e design/image-1.png>)

## Design styles
Una volta definiti i moduli, vogliamo organizzare questi. Esistono diversi stili.

### Layered system
In un sistema organizzato a **livelli** si definisce una gerarchia basata sulla relazione **use**. In particolare, ogni modulo può comunicare solo con i moduli del livello sottostante. Un esempio è la struttura a onion-ring, tipica ad esempio dei primi sistemi operativi o delle virtual machine:\
![alt text](<../!Images/17 - Modeling e design/image-2.png>)\
In questo modo i problemi di un modulo restano interni al modulo e impattano solamente i livelli sottostanti, proteggendo i livelli più esterni. Una variante di questo approccio, spesso adottata in architetture web, è la **variante relaxed**, permettendo ai layer di bypassare il meccanismo del layering, ossia un modulo può comunicare con tutti i servizi dei livelli sottostanti, senza limitarsi al solo livello più vicino.

### Pipes and filters
I moduli possono essere progettati in modo tale che non si conoscono tra di loro. Sono moduli totalmente disaccoppiati: ogni modulo è un componente che svolge un servizio prendendo un input e dando un output. Delle **pipes**, ossia dei **data flow**, trasferiscono i dati da un modulo ad un altro. Pertanto:
- i componenti sono i **filters**, che non condividono dati tra loro;
- i connettori, ossia le **pipes**, trasportano i dati da un canale di output di un componente a un canale di input dell'altro.

Esistono diversi regimi di controllo per questa architettura:
- **batch**: si processa a lotti, dando un lungo elenco di input in un file. Si processa un blocco di dati per volta;
- **sequenziale**: si processa un input attivando un solo blocco per volta. L'output è determinato da una sequenza finita di passi, per cui è consistente, garantendo anche semplicità di implementazione in quanto non si ha concorrenza; 
- **concorrente**: tutti i blocchi sono attivi contemporaneamente e non si aspetta di aver concluso tutti i passi per iniziare a processare l'input successivo. I filtri vengno eseguiti in **parallelo** garantendo maggiore efficienza e lavorando in maniera asincrona.

Poiché i moduli non si conoscono tra di loro, il meccanismo:
- permette naturalmente il riuso;
- permette di modificare in maniera facile il sistema;
- permette di definire il comportamento del sistema in virtù della composizione dei moduli.

Tuttavia, ci sono alcuni svantaggi:
- non si ha persistenza. Questo tipo di architetture consiste nella definizione dinamica della struttura, pertanto l'architettura va assemblata ogni volta;
- se un'operazione dev'essere svolta più volte, l'unica opzione possibile è replicare il filtro, causando replicazione;
- tendenza ad un'organizzazione batch. Un sistema con architettura pipes and filters se usato in organizzazioni non batch permette di avere un flusso di dati che scorre continuamente attraversso i filtri. Un approccio batch obbliga il sistema a processare un blocco di dati per volta, limitanto interattività. 

### Repository-based system e architettura blackboard
L'architettura **repository-based** può ruotare attorno ad uno strato di **persistenza di dati**. Tutti i componenti interagiscano attraverso una **repository**, e non in maniera diretta tra loro. Un componente viene interessato in virtù di un aggiornamento in repository. Questo tipo di architettura è stato già visto nel caso della programmazione reactive.

In base al tipo di repository distinguiamo:
- il modello **database**, con repository passiva e con un componente aggiuntivo che gestisce le transazioni garantendo coerenza interna;
- il modello **blackboard**, con un repository attivo. Alle modifiche di stato corrispondono letture e scritture su altri componenti. Possiamo inserire all'interno dell'architettura una serie di componenti che dal punto di vista singolo forniscono un pezzo di funzionalità e lasciamo che questi si coordino per risolvere un problema anche molto vasto. Questa architettura permette quindi di risolvere in maniera decentralizzata un problema: non esiste una sequenza di elaborazione, ma essa è dettata dallo stato del repository.

### Event-based systems
Un sistema **event-based** è una generalizzazione dell'architettura Model-View-Controller. I componenti interagiscono attraverso notifiche dirette a loro inviate da un **event manager**. Questo facilita le strategie di integrazione: i componenti non dipendono l'uno dall'altro e possono essere aggiunti al sistema facilmente. Tuttavia, si possono avere problemi di scalabilità. Infine, i componenti sono asincroni tra loro: per garantire un ordine degli eventi, si dovrebbero sincronizzare tra loro tutti gli orologi dei componenti.