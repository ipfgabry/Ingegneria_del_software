# Scrum
A differenza di XP, il framework Scrum è più "classico". Il processo di sviluppo descrive le attività, i ruoli e i prodotti di ogni attività, nonché le tempistiche da seguire per svolgere le attività. Scrum è una metodologia **organizzativa**, concentrata sugli aspetti gestionali del processo di sviluppo, seguendo tuttavia un approccio agile, non utilizzando la documentazione come mezzo di comunicazione principale ma l'interazione con il cliente.\
Alcuni aspetti in Scrum si sovrappongono con il modo di procedere che c'è in XP. Il ciclo di attività veniva iterato in XP, e questo concetto di attività è presente anche qui. 

Oggi Scrum è uno tra i framework più utilizzati. Il 66% degli sviluppatori usa solo Scrum, mentre un altro 15% utilizza Scrum insieme a XP.

Anche Scrum è un metodo iterativo. L'iterazione prende il nome di **sprint**, ed è tendenzialmente più lunga rispetto a XP. Lo sprint è lo svolgimento di un'attività che porta alla realizzazione di un goal, ossia di un obiettivo funzionale (quindi un requisito) principale nel sistema. Un requisito infatti può essere fine, ma quando si parla di goal si parla di una **macro funzionalità**, ossia di un obiettivo complessivo. Risulta normale quindi considerare uno sprint con una durata maggiore di 2 settimane, mentre con XP il ritmo dell'iterazione è di una o due settimane. **Eventi** all'interno dello sprint ne dettano il ritmo:
- il **daily scrum**, un briefing quotidiano prima di mettersi al lavoro;
- lo **sprint review**.

Questi eventi sono **time boxed**, ossia limitati a livello temporale, così come lo sprint. Ad esempio, il daily scrum deve avere una durata pari a 15 minuti.

Scrum specifica anche dei ruoli e degli artefatti. Inoltre, anche in questo framework abbiamo dei principi:
- **trasparenza**: il codice e gli artefatti sono accessibili a tutti e non si hanno livelli di accesso differenziati;
- **ispezione**: garantendo la trasparenza ci si può rendere conto se ci sono aspetti che devono essere sistemati. La potenza con cui si analizzano le informazioni non è limitata solo alle persone che hanno il privilegio di vedere il codice, ma si amplifica a tutti i membri;
- **adattamento**: poter ispezionare il codice permette di intervenire, modificano ove necessario i dati e il codice.

## Ruoli
Riconosciamo diversi ruoli all'interno del team Scrum.

### Product owner
Il product owner corrisponde al **cliente** del processo XP. Si tratta di colui che fornisce i requisiti del sistema richiesto, nonché feedback su quello che viene prodotto. Il product owner gestisce il **product backlog** degli item di lavoro, ossia i requisiti del sistema. Inoltre, imposta i goal che si devono raggiungere durante uno sprint.

### Developer
Lo sviluppatore come in XP stima il tempo per l'esecuzione di un task. Al termine di ogni sprint, dovrebbe essere sempre rilasciato un incremento del software, ossia un progresso concreto a livello dell'utente. Questo progresso concreto si traduce nell'aggiunta di funzionalità.  

### Scrum Master
Lo **Scrum Master** è un coach, ossia un moderatore e un aiutante. Si tratta di una figura presente, ma non è un capo che comanda. Si occupa di risolvere problemi per rendere più semplice lo sviluppo alla squadra. Se ci sono **impedimenti esterni**, ossia problemi non tecnici ma di qualunque natura che hanno ripercussioni nell'attività di sviluppo, egli si occupa di risolverli (ad esempio, se scade la licenza del software usato per sviluppare, lo scrum master si occupa di rinnovarla). Si assicura anche che le scadenze vengano rispettate.

Mentre XP si concentra sul produrre codice in maniera agile, di qualità ma senza una fase di design, Scrum si occupa di garantire che il processo di sviluppo arrivi al termine. Per quanto riguarda le attività di sviluppo, l'unico livello a cui ci si ferma è il livello gestionale.

## Schema del funzionamento
![alt text](<../!Images/20 - Scrum/image.png>)\
Si parte da un artefatto iniziale, che è un insieme di task da completare, il **product backlog** (letteralmente "arretrato del prodotto"). A partire dai task da completare, **si pianifica lo sprint**. Una frazione del product backlog, lo **sprint backlog**, contiene i task da svolgere nello sprint.

Ogni giorno si ha un **daily scrum** (evento) a cui partecipano gli sviluppatori e lo scrum master per la pianificazione del lavoro del giorno. Questo viene ripetuto ogni giorno durante la fase di implementazione. Si produce così un **product increment**. 

Infine, al termine dello sprint si hanno due eventi:
- **sprint review**, ossia una review dell'incremento prodotto;
- **sprint retrospective**, ossia una riunione in cui si ragiona sul come è stato svolto l'iterazione a livello progettuale. 

Nella sprint review partecipa il product owner, in quanto si parla dell'incremento di prodotto. Nella sprint retrospective il cliente non viene coinvolto.

Anche le riunioni hanno una durata prefissata. Il tutto è infatti organizzato in attività con eventi di durata prestabilita che dev'essere rispettata rigidamente. Le durate degli sprint sono sempre uguali, in quanto sono fortemente legate alla natura del sistema da sviluppare. Nella realtà empirica delle cose, la durata ottimale è sempre uguale durante lo sviluppo di un oggetto. Solitamente gli elementi che si tengono in considerazione sono di due tipi:
- a che livello di maturità è il progetto che sto intraprendendo. C'è già stata una prima valutazione del rischio? È un prodotto nuovo? Ci sono assunzioni che si possono fare in sicurezza? Oppure è un prodotto che è già esistente, e quindi quello che si fa è un miglioramento? 
- durata complessiva del progetto. L'intero product backlog in quanto tempo dev'essere svolto? **In quanti mesi posso portare a termine il progetto**?

In base a questi parametri si ha un'indicazione sulla durata dello sprint, che varia da una a quattro settimane. Questi numeri sono gli stessi in XP, solo che in XP quattro settimane sono da evitare, spingendo di più sulla singola settimana, ossia il minimo possibile.

![alt text](<../!Images/20 - Scrum/image-5.png>)

Il grafico sottostante riassume quanto detto:\
![alt text](<../!Images/20 - Scrum/image-1.png>)
### Sprint planning
Lo sprint planning è una riunione di massimo 2 ore per settimana moltiplicato per il numero di settimane per lo sprint. Lo sprint planning quindi, se lo sprint dura 2 settimane, dura al più quattro ore. Nello sprint planning lo scrum master si assicura che la scadenza del progetto sia rispettata, in modo tale che il non completamento del progetto non è un problema gestionale. Il rischio quindi non è legato alla gestione del processo di sviluppo. 

### Daily Scrum
Il daily scrum è una riunione tra gli sviluppatori e lo scrum master che assume il ruolo di moderatore. In massimo 15 minuti ogni sviluppatore deve rispondere a tre domande:
- **cosa ho fatto ieri?**;
- **cosa farò oggi?**;
- **ci sono ostacoli?**.

Eventuali ostacoli verranno rimossi dallo scrum master.

### Sprint review
Nella sprint review è presente anche il product owner. Si analizza l'incremento prodotto, ma si ha anche un feedback dal cliente, che vede il prodotto prendere forma. Poiché intanto è passato del tempo, si ha la possibilità di modificare i requisiti, aggiornando il product backlog in base al feedback dell'utente. Sebbene si proceda per incrementi, l'utente ha un product backlog modificabile nel tempo: si ha quindi un approccio ibrido tra incrementale e iterativo. 

### Scrum board
Tipicamente si usa una scrum board, che permette di gestire lo sprint backlog. Si parte dalle user stories, tracciando i task da svolgere, quelli in progress e quelli completati.

![alt text](<../!Images/20 - Scrum/image-3.png>)
### Kanban Board
In alternativa alla Scrum board si utilizza la **Kanban Board**, una metodologia agile estrema che nasce da un'esperienza industriale. La Kanvan Board è una lavagna in cui tutti possono vedere i task da svolgere. In questo modo si condivide una visione di insieme di quali sono gli obiettivi del progetto, condividendone lo stato di avanzamento.

La kanban board permette di gestire il product backlog e le attività prese in considerazione. Queste board inoltre sono più facili da adattare al contesto. 

![alt text](<../!Images/20 - Scrum/image-4.png>)

## Gestione dei problemi
Scrum mira all'organizzazione funzionale del team di sviluppo pur essendo una metodologia agile tramite un development incrementale, con una negoziazione in corso d'opera dei requisiti del sistema. A differenza di XP, in cui i requisiti scritti potevano essere modificati in quanto si ha il cliente in sede, in Scrum lo sprint goal **non è negoziabile**. Infatti gli sviluppatori potrebbero sopravvalutare le loro capacità e realizzano che lo sprint backlog non può essere implementato nella sua interezza. Allora gli sviluppatori possono rinegoziare lo sprint backlog.

## Scaling del framework
Finché il progetto è piccolo lo Scrum si adatta anche a team piccoli. Se i team sono molto grandi, tuttavia, il daily scrum di 15 minuti non sarebbe sufficiente. Si divide allora il team in sotto-team, dove in ogni team è presente uno scrum master. In ogni sotto-team si ha un daily scrum con il proprio scrum master.

## Features non presenti
Se tutto il product backlog non può essere implementato durante il tempo schedulato per il progetto, allora:
- si svolgono prima i task con priorità più alta durante lo sprint;
- **feature non essenziali che non rientrano nello schedule possono essere scartate**;
- se le feature essenziali non rientrano nei tempi previsti, si negozia con il cliente.

## Scrum e XP
Si potrebbero applicare le 12 pratiche di XP all'interno del framework Scrum. Ogni giorno dello Scrum prevede un daily scrum in cui ci si siede a lavorare. I primi giorni ci si occupa della scrittura dei test. Nei successivi si scrive il codice. Infine, si scrive il refactoring. Si passa quindi alla sprint review e successivamente allo sprint retrospective, chiudendo l'iterazione. Questa fusione permette di prendere il "buono" di XP, fornendo uno sviluppo guidato da una progettazione gestionale del progetto.

Le principali differenze sono le seguenti:
- in XP le iterazioni sono volontariamente corte, mentre in Scrum gli sprint possono essere più lunghi in virtù del progetto;
- in XP si ha una totale flessibilità con i cambiamenti, mentre in Scrum si ha più rigidità, perché i goal non vengono cambiati durante lo sprint;
- in XP il focus è sulle pratiche ottimali per dare qualità al lavoro pur essendo un lavoro di codifica svolto in un contesto di continuo aggiornamento, metnre in Scrum il focus è sul team che si organizza in modo agile ma che riesce a dare delle garanzie che le attività procederanno con le tempistiche pianificate;
- in XP il cliente determina l'ordine dei requisiti da sviluppare, mentre in Scrum il cliente decide il goal, ed è il team che decide i task del goal da svolgere prima, stabilendo delle priorità. Quindi l'ordine dei task è dettato in XP dal cliente, mentre in Scrum la priorità è un aspetto tecnico che viene gestito dagli sviluppatori.

# Manutenzione
I requisiti di sistema cambieranno mentre il sistema è in sviluppo in quanto l'ambiente cambia. Quando un sistema viene installato in un ambiente, **esso cambia quell'ambiente** e quindi cambia i requisiti di sistema. Il sistema allora dev'essere mantenuto in modo tale da rimanere **utile** in un ambiente. Le modifiche da applicare sono di tre tipi:
- modifiche **riparative**. Si scopre durante il funzionamento del sistema che esso ha dei bug o dei malfunzionamenti;
- modifiche **adattative**. Uno dei cambiamenti nel contesto operativo richiede necessariamente di intervenire nel software affinché esso continui a funzionare (y2k ad esempio);
- modifiche **additive**, se bisogna aggiungere o modificare funzionalità del sistema.

Circa due terzi delle modifiche da applicare sono di tipo additivo. 

## Costo della manutenzione
Il costo della manutenzione è tale che esso è da due a cento volte di più del costo di sviluppo. In più, questo costo dipende da come abbiamo lavorato prima, ad esempio il costo dell'aggiunta di una funzionalità dipende da come abbiamo implementato il sistema. A prescindere da quanto possa essere fatto bene, il software **invecchia**: più tempo passa, più queste modifiche aumentano il debito tecnico. Le aggiunte non possono sempre essere fatte bene, per cui inevitabilmente si va incontro ad un momento finale. 

Ciò che si risparmia quindi in tempo di sviluppo potrebbe aumentare eventuali spese di manutenzione.

## Dinamica dell'evoluzione del programma
Sono state fatte delle osservazioni in relazione all'evoluzione del programma:
- **legge del cambiamento continuo**: un sistema usato nel mondo reale dev'essere necessariamente modificato, altrimenti diventerà inutile;
- **legge della conservazione di familiarità**: i cambiamenti incrementali in ogni release sono approsimativamente costanti. Ciò che cambia è una parte sempre della stessa importanza;
- **legge dell'incremento di complessità**: la struttura del sistema diventerà più complessa, e risorse extra devono essere considerate per mantenere e semplificare la struttura;
- **legge della large program evolution**: l'evoluzione del programma è un processo auto-regolatorio, per cui gli attributi del sistema sono invariati da rilascio a rilascio;
- **legge di stabilità organizzativa**: il tasso di sviluppo di un programma è approssimativamente costante;
- **legge della crescita continua**: per mantenere la soddisfazione dell'utente, le funzionalità offerte dal sistema devono crescere continuamente;
- **legge del declino di qualità**: la qualità del sistema degrada se non viene adattato ai cambiamenti dell'ambiente in cui opera.

Queste leggi, note come **leggi di Lehman**, sono applicabili generalmente a sistemi grandi sviluppati da aziende.

## Impatto sui costi di manutenzione
I fattori che incidono sui costi di manutenzione sono di diversa natura:
- **stabilità di squadra**. Se si mantengono sempre le stesse persone che conoscono la storia del prodotto, si abbattono i prezzi di manuntenzione;
- **responsabilità contrattuale**. Se gli sviluppatori del sistema non sono coinvolti a livello contrattuale alla manutenzione del sistema, non percepiscono responsabilità;
- **team skills**. Le abilità del team influenzano la qualità del prodotto;
- **età del prodotto**. Più un prodotto invecchia, maggiori sono i costi.

## Metriche
Abbiamo bisogno di metriche per misurare la qualità del sistema. La qualità di un sistema si può esprimere sotto molti aspetti. La maggior parte di questi aspetti sono **qualitativi**, ossia sono misurati in termini non numerici (riusabilità bassa/alta, affidabilità bassa/alta, ecc.). Analizziamo ora le metriche misurabili **quantitativamente** che riguardano la complessità del sistema. "Complessità" in questo caso è un termine lasco, ossia si misura un aspetto **volumetrico** del sistema, ossia con una dimensione spaziale.

La complessità allora dipende dalle strutture di controllo e dalle strutture dati, nonché dalla struttura dei moduli.

### Righe di codice
Possiamo misurare la dimensione del sistema con le **righe di codice**. La riga di codice è una qualsiasi riga del programma che **non è un commento** o **non è vuota**. Legata alle righe di codice è la **percentuale di commenti**. Risulta molto utile commentare il software, e si suggerisce di avere una percentuale di commenti pari al 30% rispetto alla lunghezza complessiva del file. Se un file ha 100 righe di codice, almeno 30 in più dovranno essere commenti.

### Metriche di Halstead
I termini più piccoli che possiamo considerare sono gli operatori e gli operandi. A partire dal numero di operatori totali e unici e dal numero di operandi totali e unici si possono derivare le seguenti metriche:
- **lunghezza** ($N_1, N_2$): somma di operatori totali e operandi totali;
- **vocabolario** ($n_1, n_2$): somma di operatori unici e operandi unici;
- **volume**, definita come la lunghezza del programma moltiplicata per il logaritmo in base due del vocabolario. Questo valore dovrebbe essere compreso per una funzione tra 20 e 1000, mentre per un singolo file da 100 e 8000. Questi numeri sono stati studiati dalla VerifySoft;
- **difficoltà**, definita come $D = \frac{n_1}{2}\cdot \frac{N_2}{2}$;
- **effort**, definito come $E = V\cdot D$;
- **tempo stimato per compresione**, definito come $T = \frac E{18}$;
- **bug consegnati**, definito come $\frac{E^{\frac 23}}{3000}$

Queste metriche sono **insensibili** a come scriviamo il codice. Infatti un `if` può essere scritto, ad esempio, in un'unica riga, mentre in questo caso si ha un'indicazione che non dipende da quante volte andiamo a capo, ma solo quali e quanti operatori e operandi usiamo. Queste metriche, inoltre, derivano da calcoli basati su interpolazione. 


### Complessità di McCabe
La **complessità di McCabe** corrisponde alla **complessità ciclomatica del grafo di controllo di flusso**. Questo numero esprime il numero di percorsi per eseguire tutte le funzionalità rappresentate dal grafo, e quindi fornisce il numero minimo di test ed è una misura della complessità del software.

Chiaramente è ovvio che il numero di McCabe **non corrisponde al numero totale di percorsi del grafo di controllo di flusso**.

### Indice di manuntenibilità
Sulla base di interpolazioni si possono mettere in gioco il volume di halstead, le righe di codice e la complessità di McCabe ($V(g)$) per calcolare l'**indice di manuntenibilità**:

$$MI_1 = 171-5.2\ln(V)-0.23V(g)-16.2\ln(LOC)$$

Questo numero viene "corretto" utilizzando una funzione che da un peso in virtù del numero di commenti presenti nel codice: 

$$MI_2 = MI_1 + 50\sin{\sqrt {2.46\cdot CP}}$$

dove $CP$ indica la percentuale di commenti nel codice. Solitamente valori sopra 85 sono molto positivi e indicano che il codice è facilmente mantenibile.

### Metriche di Chidamber e Kemerer
Possiamo usare altre metriche specifiche per un sistema Object-Oriented.

#### Weighted Methods per Class (WMC)
Data una classe $C$ con metodi $M_1, M_2, ..., M_k$ con relative misure di complessità per i metodi $c_1,c_2,...,c_k$, si ha che la **Weighted Methods per Class** è data dalla somma $c_1+c_2+...+c_k$. Se come misura si utilizza la metrica "identità", allroa la somma esprime solo il numero di metodi presenti. Più grande è il WMC maggiore è l'impatto sulle classi che ereditano questa.

Un sistema di alta qualità vede una curva della WMC con una forte distribuzione verso l'origine dell'asse $y$. Empiricamente inoltre si è anche visto che WMC è correlato al numero di bug.

![alt text](<../!Images/20 - Scrum/image-6.png>)

Nel grafico soprastante notiamo che esiste un numero di classi pari a zero con una WMC alta, il che significa che maggiore è il numero di metodi, maggiore è la probabilità di avere errori.

#### Profondità dell'albero di ereditarietà (DIT, Depth Inheritance Tree)
Se ci sono classi che hanno tanti predecessori, l'albero dell'ereditarietà sarà più profondo, indicativo di un sistema più complesso. Empiricamente si è osservato che è meglio evitare valori superiori a 5.

Chiaramente bisogna decidere come dev'essere interpretata la profondità in virtù dell'importazione di librerie. In effetti potremmo implementare una libreria, che a sua volta estende funzionalità da un'altra liberia, che a sua volta estende altre funzionalità di un'altra libreria. Bisogna quindi decidere se:
- includere nella metrica **tutte le librerie coinvolte nell'ereditarietà**;
- includere nella metrica **solo le librerie importate**;
- **non includere le librerie importate**.

#### Number of children (NOC)
La metrica **number of children** conta il numero di sottoclassi di una classe. Un NOC alto potrebbe essere indicativo di un'alto riuso della classe base. Oppure potrebbe essere un segnale, se il NOC diventa troppo alto, che il sistema è progettato male: la superclasse potrebbe essere troppo astratta, può essere messa in comune con molte cose in quanto troppo generica. Quindi l'astrazione è errata. Empiricamente un NOC alto è indicazione di minori fault nel sistema.

#### Coupling Between Objects (CBO)
L'accoppiamento tra classi è inteso come l'utilizzo di una variabile o di un metodo di un'istanza di un'altra classe all'interno del codice della classe presa in considerazione. Si individua allora quant'è il numero di classi che sono accoppiate: tale numero è il **Coupling Between Objects**. Il CBO può essere calcolato con:

$$\text{CBO} = \text{FanOut} - \text{ FanIn}$$

Dove FanOut e FanIn sono rispettivamente il numero di classi usate dalla classe analizzata e il numero di classe che usano la classe analizzata.

Un alto CBO è sconsigliato, in particolare un CBO superiore a 14 previene il riuso. Inoltre, il mantenimeno è più difficile, visto che singole modifiche alle classi influenzano le altre.

#### Response For a Class (RFC)
Quando si invoca il metodo di una classe, il codice del metodo può invocare il codice di un metodo di un'altra classe. Il **Response For a Class** è il numero di metodi locali di una classe sommato al numero di metodi remoti chiamati da altri metodi (nota: la somma non dev'essere reiterata. Ad esempio, se m1->m2->m3, allora RFC per la classe con m1 è pari a 2, non a 3). RFC esprime quanto codice viene eseguito da una classe quando viene invocato un metodo. Più è alto RFC, maggiore è la probabilità che ci siano errori e la difficoltà nel debugging e nel testing, nonché la complessità della classe.

#### Lack Of Cohesion in Methods (LCOM)
La metrica **Lack of Cohesion in Methods** misura la coesione all'interno dei metodi. Indichiamo con $P$ le coppie di metodi distinti in una classe che non condividono variabili di istanza. Indichiamo con $Q$ le coppie di metodi distinti in una classe che condividono variabili di istanza. Allora:
- se $P > Q\implies LCOM = P-Q$;
- se $P \le Q \implies LCOM = 0$.

Una seconda versione di LCOM permette di ottenere valori anche negativi, definendo $LCOM = P-Q$. Anche con questa leggera miglioria la metrica è ancora migliorabile. Infatti la metrica non ci fornisce indicazioni su quanto è grande un gruppo di metodi che non sono coesi.\
Indicando con $v$ il numero di attributi, $m$ il numero di metodi e $m(V_i)$ il numero di metodi che accedono a $V_i$, definiamo:

$$LCOM = \frac{\frac 1v\cdot \sum_{i=1}^v m(V_i)-m}{1-m}$$

Il valore di LCOM in questo caso è compreso tra 0 e 2. Quando la coesione è massima, tutti i metodi utilizzano tutte le variabili, ossia $m(V_i) = m$. Si può superare il valore 1 quando un metodo non utilizza alcun metodo: in questo caso si ha infattti che $m(V_i)$ = 0. All'estremo nessun metodo utilizza nessuna variabile, per cui dalla somma si ottien $m-\frac 1{m-1}$, che è minore o uguale a 2. 

Neanche con questa formulazione tuttavia riusciamo a distinguere situazioni in cui si hanno gruppi separati di metodi che utilizzano un sottoinsieme di variabili. La metrica non fornsice informazioni aggiuntive neanche in questo caso:\
![alt text](<../!Images/20 - Scrum/image-2.png>)\
Nell'esempio `A` è meno coeso di `B` e `C`.

Si possono rappresentare le dipendenze con un grafo. Ogni nodo è un metodo e un arco indica che i due metodi accedono alla stessa variabile. Allora LCOM indica il **numero di componenti fortemente connesse nel grafo**. In particolare:
- con 0 non si hanno metodi;
- con 1 si ha una componente coesa;
- con 2 o più si ha mancanza di coesione.

#### Violazione di Demetrio
La legge di Demetrio nel contesto della OOP è semplicemente la legge dell'**utilizzo di un solo punto**. Ossia, basta fare `b.m()`, e non `b.c.m()`. Nel secondo caso, significa che un oggetto che si interfaccia con `b` deve conoscere anche l'interfaccia di `c`. Questa dipendenza non è voluta, in quanto è esplicita. Le dipendenze indirettte permettono di far dipendere un livello solo da quello successivo, cosa che nel caso `b.c.m()` non è vera.