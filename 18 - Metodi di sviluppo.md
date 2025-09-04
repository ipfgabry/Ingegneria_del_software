# Metodi di sviluppo
Quando il software ha cominciato a crescere sia in complessità sia in presenza nelle applicazioni quotidiane, ci si è resi conto che sviluppare i progetti software in maniera "artigianale" e basandosi sulla capacità del singolo non era affidabile. Si utilizzano oggi dei metodi per minimizzare i rischi nello sviluppo.

Solitamente il rischio era inteso come la possibilità di "sbagliare" prodotto, ossia che il destinatario del prodotto non era soddisfatto. I motivi tuttavia per cui un progetto può fallire vanno anche oltre errori di questo tipo; tra questi vi sono le carenze nella metodologia per il raggiungimento del risultato finale, come la progettazione dello sviluppo.

## Processo
Diamo qualche definizione per i termini tecnici utilizzati.

Definiamo un **processo** come un insieme di **attività orchestrate**, ossia modi in cui si organizzano una serie di attività. Durante queste attività vengono prodotti dei risultati concreti, che possono essere di natura diversa (simulazioni, documenti, codice). Un processo deve essere in grado di identificare i **ruoli** e le **abilità** necessarie per l'esecuzione delle attività.\
L'**attività** è un lasso di tempo durante il quale si lavora per la produzione di un risultato concreto a partire da **artefatti** in ingresso. Per ogni attività è necessario avere un criterio per misurare i progressi. Questo chiaramente è un problema che a livello di processo di traduce nel monitorare i progressi da un'attività ad un'altra.\
Distinguiamo un altro tipo di elemento all'interno di un modello da descrivere, ossia l'**evento**. L'**evento** è qualcosa di completamente asincrono, indipendente dalla fine di un'attività.

### Esempio: processo di sviluppo iterativo
![alt text](<../!Images/18 - Metodi di sviluppo/image-4.png>)\
In questo schema ogni attività è un riquadro. La dipendenza delle attività la rappresentiamo con gli archi.
Lo schema è organizzato come un ciclo. L'idea dietro questo processo di sviluppo è che il lavoro procede in maniera **iterativa**, ripetendo la stessa sequenza di attività più volte. Nello schema la dicitura "Deliver code base" non è un evento. Se sono output o input di un'attività che provengono dall'interno, questi non sono eventi. Il rilascio del codice è un'output di un'attività prodotto dal sistema, e non è in se un evento. L'evento in questo caso è "start", che viene dall'esterno: è un momento asincrono la cui tempistica non è determinabile in relazione agli altri elementi del processo.

## Attività
Quando pensiamo ad un'attività, possiamo scoprire diversi tipi di obiettivi. Alcuni tipi di attività sono:
- analisi dei requisiti;
- progettazione (design);
- implementazione (codifica);
- convalida (testing e approvazione);
- rilascio e manutenzione operativa (deployment e maintenance).

Alcuni approcci di sviluppo sono focalizzati sul rilascio del prodotto in tempi ottimali. Ma il vero obiettivo non è solamente produrre un sistema funzionante, ma anche far si che il sistema viva a lungo. La fase di manutenzione è la fase più renumerativa nel tempo. Infatti ad esempio un progetto può essere sviluppato in 3 mesi o più, ma l'azienda che sviluppa il progetto viene pagata per mantenere in vita il progetto.

## Classe di processo
In teoria siamo anche in grado di dare indicazioni più astratte, dando **classi di processo**. Ossia, si forniscono metodologie, non dicendo esplicitamente quali attività selezionare, ma fornendo uno stile di sviluppo da seguire. Alcuni esempi sono il modello iterativo e il modello a cascata.
## Framework
Definiamo come **framework** il quadro d'insieme in cui alcuni dettagli possono essere liberi. Si ha una specifica parziale dell'impostazione da dare al processo di sviluppo. Alcuni esempi sono le metodologie Scrum e Spiral.
## Practice
Le metodologie che non sono un processo in senso tecnicamente corretto vengono chiamate **pratiche**. Le pratiche non dicono dettagliatamente quali sono le singole attività e come si debbano comporre, ma semplicemente quali sono le tecniche concrete da applicare.\
Le **best practices** sono dei modi corretti ben precisi per portare a termine ogni attività. Sono quindi indicazioni pratiche su come svolgere le attività, fornendo a livello pratico le metodologie piuttosto che a livello gestionale. Non si vede lo sviluppo come inquadrare l'attività in uno schema, ma si lascia libera la selezione dell'attività, purché si seguano le linee guida fornite.

## Processo a cascata (Waterfall)
La caratteristica distintiva del **processo a cascata** è l'estrema sequenzialità. Si divide l'attività progettuale in una serie di attività distinte tra loro, ma l'idea è di azzerare l'interazione tra le attività. Le componenti interagiscono solo attraverso gli artefatti passati alla fase successiva.

Il procedimento nasce con delle frecce da sopra a sotto. Successivamente ci si è resi conto che il processo serve solo per un progetto di grosse dimensioni in cui è importante e costoso portare a termine ogni singola attività. In questi progetti è importante che non ci siano errori. Correggere uno sbaglio commesso in una delle attività può essere costoso. Pertanto, vogliamo che ogni **singola attività produca artefatti privi di errore**, ad esempio assegnando ad ogni attività dei team specializzati. Fintanto quindi che il progetto è ad alti costi, per cui ha senso che le attività vengano svolte con altrettanto impegno e allo scopo di garantire che il risultato sia corretto, il meccanismo funziona. Il modello a cascata garantisce un risultato corretto quindi se:
- il sistema è costoso;
- il tempo a disposizione è abbondante;
- **i requisiti temporali non cambiano**.

![alt text](<../!Images/18 - Metodi di sviluppo/image.png>)\
Questo tipo di approccio tuttavia non è buono se i requisiti cambiano. In più, c'è il rischio in questo approccio che qualche errore possa essere commesso. Ciò rappresenta una vulnerabilità implicita nel modello a cascata. Si aggiunge allora un meccanismo di **feedback** con le fasi precedenti. L'interazione con le attività è quindi sia "in avanti" che "all'indietro":\
![alt text](<../!Images/18 - Metodi di sviluppo/image-1.png>)\

Distinguiamo allora due estremi:
- **one-shot**, ossia un meccanismo puramente sequenziale;
- **feedback**, garantendo che ogni attività possa tornare indietro, svolgendo il processo di sviluppo andando avanti e indietro tra le varie attività fino a quando non si arriva alla fase conclusiva. 

### Svantaggi 
Un primo svantaggio riguarda la componente temporale: il prodotto risultante infatti può essere ottenuto dopo un grande lasso temporale. Inoltre, si ha poca interazione con il cliente, in quanto si ha solamente nella fase di richiesta dei requisiti. La poca interazione con il cliente comporta anche la difficoltà ad introdurre cambiamenti richiesti da quest'ultimo.

### Vantaggi 
Il modello a cascata garantisce una **consistenza** tra gli artefatti passati alle attività. Ogni attività, inoltre, produce del materiale che resta come **storico**, ossia come **prova di avanzamento** del progetto. Questo storico può anche essere un insieme di materiale intermedio, non obbligatoriamente presente nel prodotto finale, ma garantisce **documentazione**.\
Il modello a cascata è particolarmente potente quando **i requisiti non cambiano**, garantendo la produzione di un prodotto di **altissima qualità**. 

## Processo evolutivo
Chiaramente non tutti i processi sono grandi o con disponibilità economiche e temporali infinite. Passiamo allora all'estremo opposto: il **processo evolutivo**.\
Si realizza uno sketch, ossia un outline del sistema da sviluppare, e si evita di ritardare l'inizio della codifica del sistema. Essenzialmente, **si realizza il sistema in molteplici versioni** in virtù delle specifiche finali. L'idea del processo è la seguente: si ottengono dei requisiti chiari, e il cliente guida la sequenza delle evoluzioni tramite un meccanismo **build and fix**. L'idea è quindi quella di non dover richiedere requisiti finali, ma di avere un'interazione continua con il cliente che si basa sul confrontarsi con il cliente in virtù dei prodotti intermedi. Il numero di interazioni con il cliente è indefinito, e dipende da quando il cliente è definitivamente soddisfatto.

![alt text](<../!Images/18 - Metodi di sviluppo/image-2.png>)

### Svantaggi
La tempistica di questo processo è **assolutamente imprevedibile**. Il waterfall prevede uno studio degli obiettivi, per cui è possibile stimare gli effort per le attività, e quindi il tempo complessivo. In questo caso **non si ha idea di quando verrà completato il progetto**. Inoltre, in questo processo il lavoro è svolto su informazione informale, senza richiesta di cura dell'input del cliente. Quello che viene svolto **non è ben documentato**: viene solamente mostrato come risultato intermedio al cliente in virtù di ulteriori specificazioni. Questo modus operandi pertanto non è ben documentato. Infine, **è assente la visione di insieme**.

### Vantaggi
Per sistemi estremamente piccoli, in cui tutto sommato quello che si mette in gioco è semplice e non complesso, i difetti dell'approccio non sono pesanti. Il fatto che i requisiti sono trattati con superficialità e che il lavoro di sviluppo è fatto senza particolare cura non sono problemi per il prodotto finale: un'analisi più approfondita non è richiesta. Il numero di interazioni a livello gestionale è ridotto, e appena si ottiene un risultato intermedio il prodotto viene mostrato al cliente per eventuali correzioni o per il rilascio.\
Pertanto, il processo evolutivo è molto buono per progetti piccoli, per piccole parti di sistemi più grandi e per sistemi a vita breve.

## Sviluppo incrementale, sviluppo component based
Se il lavoro è complesso, possiamo gestirne la complessità suddividendolo in parti. Possiamo quindi realizzare progetti più complicati, ma con il vantaggio di avere più release, ossia più momenti per confronti con l'utente finale. La **classe di processi incrementali** prevede quindi la realizzazione del progetto tramite aggiunta di funzionalità fino al completamento, ottenendo feedback su ogni elemento aggiunto.\
Simile allo sviluppo incrementale vi è lo sviluppo **component-based** (**CBSE**, Component Based Software Engineering). I componenti sono diversi tra loro, ma a basso livello si somigliano tutti. I pezzi che servono per creare sistemi specifici alla fine sono tutti ottenuti utilizzando componenti a basso livello uguali. Basta allora prendere questi componenti, detti **COTS** (**Components Off The Shelf**), e assemblarli. 

Chiaramente i componenti non sono modificabili in quanto preesistenti. Tuttavia, è possibile **modificare i requisiti**, realizzando pertanto un sistema che si avvicina molto al risultato finale sebbene non sia precisamente identico a quello richiesto. In questo caso è possibile realizzare degli adattatori, già visto ad esempio con l'Adapter design pattern.
## Sviluppo iterativo e incrementale
L'approccio incrementale è utile quando conosciamo già qual è il risultato finale. In un procedimento iterativo, invece, ripetiamo il lavoro più volte, poiché non abbiamo le idee chiare su quello che dobbiamo fare, ma un'idea abbozzata, migliorando il prodotto fino a quando il cliente è soddisfatto. Il processo evolutivo rientra nella categoria iterativa.

Possiamo tuttavia **combinare le tecniche iterativa e incrementale**. Partiamo da un'idea superficiale, e dividiamo il lavoro in parti, iterando incrementi sulle parti da sviluppare.

## Rischi relativi al progetto
L'ingegneria del software si è evoluta molto rapidamente, e oggi si considerano molte variabili in relazione al progetto:
- ci sono aspetti legati a chi partecipa al progetto. Bisogna ad esempio garantire che il team sia soddisfatto nel lavoro di squadra;
- il team dev'essere assemblato in modo tale che ogni elemento della squadra sia coordinato, allocando le risorse necessarie. Tra queste risorse ci sono le persone, che umanamente devono essere valide e con le competenze giuste. Mettere insieme competenze non adeguate per il problema da risolvere è un rischio per il progetto, così come coordinare male la squadra;
- bisogna garantire che le tecnologie impiegate siano adeguate per la realizzazione del prodotto. Poiché un ingegnere del software non è solo un ingegnere ma anche in parte uno scienziato (inteso nell'accezione di computer scientist), può essere necessario creare gli strumenti necessari. Un altro problema è la **retrocompatibilità**: la base del sistema sviluppato, come un sistema operativo, può essere aggiornata e può potenzialmente invalidare il funzionamento del sistema;
- ci sono dei constraint da rispettare, tra cui le risorse economiche e fisiche, nonché risorse temporali.

Queste problematiche vengono affrontate tramite la **pianificazione del lavoro**, che include non solo la metodologia di sviluppo, ma anche le tempistiche e i costi. Oltre ai diagrammi use case vengono anche utilizzati i **diagrammi di Gannt**, dove sull'asse $x$ è presente il tempo e sull'asse $y$ le attività da svolgere. Si fornisce quindi un'indicazione temporale per ciascuna attività, collocandole nel tempo. Si concretizza quindi la metodologia di sviluppo mappata su vincoli di tempo e denaro. Ogni barra nel diagramma infatti rappresenta un **costo orario** che dipende dall'attività stessa. In un contesto aziendale questo permette anche la quantificazione dei costi: se 10 persone devono lavorare ad un'attività per un mese, ciò si traduce in un costo economico pari a 10 paghe mensili.

## Framework a spirale (Spiral framework)
Il primo approccio proposto per la realizzazione dei progetti è il **framework a spirale**. Si tratta di un modello iterativo, con uno schema che prevede sempre un ciclo di attività ripetute. Gli assi indicano che c'è un parametro che cresce di intensità; di nostro interesse è l'asse $y$. Quello che cresce è in questo caso il **rischio**: il rischio cresce man mano che si va avanti nello sviluppo. Più si va avanti, maggiore è l'impiego di risorse e quindi maggiore è il rischio che un errore causi costi maggiori. Si parte quindi dal centro, con rischio zero:\
![alt text](<../!Images/18 - Metodi di sviluppo/image-3.png>)

L'asse $x$ permette una divisione in quattro quadranti. Il primo, a partire dal quadrante in alto a sinistra, prevede la fase di analisi dei requisiti, che non è un'attività che dev'essere conclusa come nel modello a cascata, ma come nei modelli di tipo iterativo prevede una reiterazione. Successivamente, il quadrante in alto a destra è il quadrante che rappresenta le attività che portano all'identificazione delle alternative più o meno rischiose. Si valuta quindi l'alternativa con rischio minore, abbozzando una prima versione del sistema (nello schema è rappresentanto con la dicitura "Prototype 1"). Il prototipo realizzato viene analizzato a livello matematico, ossia utilizzando delle simulazioni, dei modelli e dei benchmark. Passiamo cosi al successivo quadrante (in basso a destra), che prevede le fasi di verifica del prodotto. Infine, è necessario eseguire una pianificazione, sia a livello di requisiti che a livello di ciclo di vita, passando nel quadrante in basso a sinistra. Il ciclo viene ripetuto, ridefinendo nuovamente gli obiettivi, valutando il rischio e sviluppando un nuovo prototipo, eseguendo ulteriori simulazioni e veificando che i requisiti siano verificati. Eseguiamo ora un piano di sviluppo, e ripetiamo nuovamente la fase di analisi. Il processo continua, realizzando un ulteriore prototipo, fornendo stavolta un'architettura al sistema e pianificando i meccanismi di integrazione del sistema e di testing. Un'ultima fase prevede lo sviluppo del prodotto finale, con un prototipo funzionale, un design dettagliato e la fase di codifica e di testing.

### Vantaggi e svantaggi
È evidente come l'approccio sia orientato alla gestione del rischio. Si ammette la possibilità che i requisiti possano cambiare nel tempo, ma non durante la fase di sviluppo: una volta cominciata la codifica, non si torna indietro. Tuttavia, si fa appoggio al fatto che effettivamente i requisiti sono stati analizzati ben quattro volte.\
L'approccio inoltre fa appoggio ad un'analisi per lo più gestionale, dando un'importanza allo sviluppo del software solamente in fase finale. Questo tipo di approccio tuttavia ritarda lo sviluppo del software fino a quando non si è sicuri dei requisiti, richiedendo quindi una risorsa temporale non indifferente. Inoltre, il framework non è del tutto foolproof a cambiamenti nella fase finale di codifica.

