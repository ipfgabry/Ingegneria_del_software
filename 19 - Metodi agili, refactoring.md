# Metodi agili
Negli anni 2000 si è notato che è necessario avere una certa capacità a reagire al mondo esterno, evitando di chiudersi sull'idea che una meticolosa progettazione possa escludere ogni tipo di problematica del mondo reale. Quello che deve avere importanza non è quello che viene documentato, ma la realtà, ossia l'interazione con il cliente. La documentazione più dettagliata in questo caso è il codice. 

Il **manifesto della programmazione agile** enuncia alcuni principi dei metodi detti **agili**. L'approccio agile evita l'utilizzo di piani, che vincola il programmatore e crea problemi, e supporta la libertà. Le informazioni devono provenire dal cliente, cercando di collaborare con questo e non bisogna negoziare per un prodotto che non sia al 100% identico a quello richiesto dal cliente. Si ha quindi la necessità di poter rispondere al cambiamento, e non di seguire ciecamente un piano.

## Extreme Programming
Il concetto dell'approccio **Extreme Programming** (**XP**) nasce duranto lo sviluppo di un progetto della casa Chrysler. All'interno del processo di lavoro vennero individuate le buone pratiche, vennero raccolte insieme e venne massimizzato l'utilizzo di queste, imponendo di fare le cose in quel modo. Il modus operandi quindi viene definito dalla pratica corretta, e tale pratica dev'essere un comandamento a cui obbedire sempre. Non basta quindi che qualcuno sia bravo, ma bisogna estremizzare l'uso dei modi migliori di fare le cose. 

Il concetto quindi non è quello di programmare in maniera estrema, ma di estremizzare l'uso delle pratiche corrette. Oltre all'utilizzo delle pratiche da applicare costantemente, l'idea è che quello che il processo XP dà non è definito da uno schema come quelli visti, ma piuttosto è una definizione. Un progetto segue l'approccio XP quando si produce una nuova release del sistema, dove per release si intende qualcosa che si installa sul posto e che il cliente può utilizzare. 

I punti chiave di XP sono i seguenti:
- **focus sul codice**: non si dà importanza al piano di sviluppo, ma allo sviluppo stesso. L'unica attività principale è la codifica, che si porta appresso attività complementari. Quindi ci sono quattro tipi di attività durante il processo XP, che sono:
  - **listening**, ossia ascoltare cosa vuole l'utente dal sistema;
  - **testing**, centrale per lo sviluppo;
  - **coding**, in particolare una scrittura di un codice semplice;
  - **design**, per ripulire il design e l'implementazione del sistema.
  L'unica documentazione è il **codice**;
- **orientato sulla gente**: la conoscenza sul prodotto è nelle persone che sono applicate a quel progetto e scambiata oralmente. Questo quindi implica un'interazione continua;
- **leggerezza**: non si hanno piani o organizzazioni, ma le persone lavorano sul progetto, rimuovendo i costi aggiuntivi e creando un prodotto di qualità con test rigorosi. Si identificano quindi gli elementi chiave e le pratiche da applicare costantemente.

### Ciclo di sviluppo XP
Per schematizzare lo sviluppo XP, basta utilizzare il seguente schema:\
![alt text](<../!Images/19 - Metodi agili, refactoring/image.png>)\
Il cliente viene ascoltato per acquisire informazioni sul sistema. Questo viene espresso sotto la nozione di **user stories**. A partire dalle stories vengono scritti dei test per le user stories, scrivendo il codice del sistema per superare i test (seguendo quindi uno sviluppo Test-Driven). Infine, si applicano i desgin pattern e un refactoring per scrivere in maniera più pulita il codice. In maniera incrementale il sistema viene prodotto tramite continue interazioni con il cliente.

Questo tipo di approccio permette di implementare il concetto di mappare le funzionalità dentro unità di codice. La scrittura infatti delle storie è importante e permette di realizzare dei moduli ben definiti. La scelta di come inserire gli incrementi tuttavia ha una valenza per il progetto. Inoltre, notiamo che nello schema è presente un arco diretto tra la scrittura dei test per le storie e il refactoring del sistema. Infatti non è detto che le nuove storie prevedano nuove codifiche, ma semplicemente il refactoring del testo. 

Il processo XP consiste nel ripetere più volte i passaggi, fino a quando non si ha un'ultima release. Ma questa ultima release non è detto che esista. Questo approccio permette lo sviluppo continuo di sistemi, aggiungendo sempre più funzionalità.

La fase di design è abbastanza trascurata. Non si ha infatti un design complessivo con una visione completa dei moduli da organizzare, ma solo un design per lavorare per un tempo limitato. La codifica di questo approccio inoltre non è proprio la codifica ideale. Idealmente vorremmo avere la definizione di un metodo e scrivere un codice che implementi nella maniera migliore il codice. Qui no: si cerca di scrivere un codice minimale, ossia una soluzione rapida per l'implementazione della funzionalità. Lo sforzo non è necessariamente giustificato e il tempo è limitato: l'obiettivo non è la qualità del codice, ma la **consegna** del prodotto.\
Più importante tra tutti è il **testing**, che fornisce le specifiche del sistema. Lo sviluppo, come detto, è **test-driven**, in quanto il codice da scrivere deve soddisfare i test. 

L'architettura globale del software soffre di questo approccio. La poca attenzione sul design si fonda sull'idea che una fase di design eccessivamente dettagliata può portare ad effort sprecati. Tuttavia, troppo poco design può portare alla necessità di un rework del codice del sistema.\
Un compromesso in questo caso è modificare il ciclo di sviluppo XP includendo un minimo di ragionamento sul design, ossia un **upfront design**, che non funge da obbligo ma da indicazione di massima. 

### Listening
L'attività di listening è importante. Il risultato dell'ascolto dell'utente sulle specifiche del sistema sono le **stories**. Queste stories descrivono uno scenario di utilizzo, non una categoria di funzionalità. Ci si adegua a quello che oggi è un template di base:
> As a \<role> I want to \<action> so that \<benefit>

Ad esempio: "Come **cliente** vorrei **prelevare soldi da una ATM** così che **posso avere contanti anche quando la banca è chiusa**".

Queste stories vengono annotate su foglietti di carta piccoli in modo tale che ogni scenario sia abbastanza piccolo ma che comunque sia in grado di descrivere le esigenze dell'utente. 

### Testing
Dalle stories deriviamo i requisiti. Per verificare i requisiti, utilizziamo dei **test** che possono essere di più tipi:
- test per verificare il funzionamento del codice;
- test che derivano dai requisiti ricavati dalle user stories (ossia test di accettazione).

I test sono strutturati con una forma che abbiamo già visto con **Mockito**:
> Dato un \<contesto> quando \<evento> allora \<risultato atteso> 

Si possono usare anche connettivi logici per concatenare contesti (ossia far sì che si verifichino più condizioni assieme). 

### Refactoring
L'idea nella fase di implementazione è di scrivere un codice che non sia il migliore ma che sia rapido da scrivere. Dopo la scrittura di codice, una fase di **refactoring** ci permette di preservare la correttezza di quanto scritto, garantendo tuttavia modularità al nostro sistema, facendo sì che il principio di singola responsabilità sia garantito.\
In generale, prima di passare alla prossima user story cerchiamo di dare una pulita al codice, in particolar modo:
- cerchiamo di applicare i design pattern;
- cerchiamo di migliorare la leggibilità;
- cerchiamo di dare nomi comprensibili per le variabili in virtù del loro scopo.

### XP e complessità
La fase di design in XP dipende dalla complessità del prodotto e dalla stabilità dei requisiti del sistema:
- con un sistema dai requisiti stabili e con un prodotto semplice, basta un design minimale per stabilire la direzione di sviluppo;
- con un sistema dai requisiti mutevoli e con un prodotto complesso, si parte da un design modificabile e modulare;
- con un sistema che cambia molto ed è semplice, si dirottano le decisioni quando necessario;
- con un sistema che non cambia ma è complesso si pongono le proprietà del sistema come design up-front.

### CRC cards
Le **CRC cards** (**Class Responsibility Collaboration cards**) documentano l'architettura in XP ed è l'unico altro tipo di documentazione accettata. Semplicemente vengono annotate:
- il nome della classe;
- le responsabilità della classe;
- le classi con cui si collabora.

Questo permette un confronto rapido e funge da class diagram informale.

## Best Practices per XP
In totale ci sono 12 osservazioni su 12 aspetti diversi che costituiscono il cuore della metodologia XP. La metodologia XP può essere incarnata in altri modi. Ad esempio, Scrum organizza il lavoro in attività a livello gestionale, che trascura il modo in cui ci si occupa del software e rappresenta un modo complementare di organizzare il lavoro rispetto a XP. 

### Gioco di pianificazione
Il cliente scrive le stories che fungono da requisiti e assegna una priorità in termini di valore. Gli sviluppatori poi per ogni iterazione dividono ogni storia in task (se è necessario, chiaramente. Una certa funzionalità potrebbe richiedere un solo task, mentre altre più task), ne valutano il **rischio** e il **tempo di sviluppo**, dando priorità ai task più rischiosi. Il rischio viene indicato con un valore numerico, e non ha a che vedere con il **rischio gestionale del progetto**. I clienti scrivono stories per coprire da due fino a quattro settimane di lavoro. Infine, il cliente e gli sviluppatori stabiliscono quando pubblicare le release. I rilasci tuttavia devono essere a ritmi rapidi, seguendo sempre l'idea di programmazione agile, per cui solitamente si opta con ritmi di 1 o 2 settimane.

Notiamo come questo tipo di operatività si basa sull'**empirismo**, ossia sulla praticità. 

### Piccole release
Solitamente ad ogni iterazione si pubblica una release. Quelle che noi solitamente intendiamo come release sono le **major release**, mentre in questo caso ad ogni iterazione si pubblica una **minor release**, mentre le major sono concordate con il cliente in virtù delle funzionalità aggiunte.

Le piccole release hanno un impatto sul design del sistema, specialmente per il fatto che non esistono pianificazioni a lungo termine (e neanche a breve termine in alcuni casi). 

### Pair Programming
Svolgere task in coppia è considerata una pratica migliore rispetto a lavorare singolarmente. In generale avendo più persone è possibile parallelizzare il lavoro e ottimizzare i tempi.\
Nel caso di XP tuttavia ogni persona decide quali story card implementare e se ne prende la responsabilità. Esistono dei micro-team, ossia delle persone si scelgono un compagno di lavoro per lo svolgimento di un task. I compagni si dividono in ruoli:
- uno svolge il ruolo di **driver**, ossia si occupa dell'implementazione dei metodi e di come questa può essere fatta nel migliore dei modi;
- l'altro svolge il ruoto di **navigator**, ossia si occupa di revisionare il codice, di scrivere i test e di vedere se tutto può essere fatto nella maniera migliore.

Sulla carta l'idea sembra molto sensata, anche perché non si lavora da soli. Tuttavia questo è vero quando c'è più armonia nella coppia: la componente comportamentale, infatti, può minare il lavoro del team.

### Metafora
Una volta che abbiamo preso l'indicazione dei clienti, abbiamo le story card e abbiamo diviso i task per coppie di programmatori, tutto deve confluire insieme in un unico prodotto. Ci si deve quindi confrontare alla fine dell'iterazione anche col cliente. Ci dev'essere un modo di **comunicare** quello che abbiamo implementato. Finché la comunicazione è tra gli sviluppatori non vi è un problema; ma con il cliente, è necessario usare **metafore per il linguaggio**. 

Sebbene sembri una cosa di secondaria importanza, in realtà non è così. È vero che basta scrivere il codice per soddisfare i test, ma i nomi e le metafore che usiamo per descrivere cosa fa il codice devono essere scelti con cura, in modo tale da evitare che nascano **problemi di incompresione**.

### Testing
I test sono i primi ad essere scritti, anche prima del codice. Si trasformano le story card in test di accettazione, in quanto le story card fungono da requisiti richiesti dal cliente.

Prima di poter dire che il task che bisogna sviluppare è pronto, bisogna eseguire i test. Ma lo step successivo chiaramente è l'integrazione all'interno del sistema da sviluppare.

### Progettazione semplice 
La fase di progettazione è una fase semplice, che evita complessità inutili per mantenere il codice chiaro e leggibile ed evitare che la fase di codifica sia rimandata, a differenza di framework come il framework a spirale che rimandano l'implementazione del sistema a favore di una meticolosa progettazione dei rischi.

### Integrazione continua
La tentazione sarebbe di integrare i pezzi del software quando si finisce l'iterazione. Questo tuttavia non aiuta lo sviluppo: se bisogna riunirsi, si potrebbero ritardare i tempi per evitare di integrare parti incomplete nel software.

Ogniqualvolta si sviluppa un pezzo di task, e questo pezzo supera i test, lo si installa nel sistema, **anche se l'iterazione non è completa**. Risulta quindi importante aiutare gli sviluppatori fornendo gli strumenti per automatizzare il testing. Ad esempio, Maven si integra negli ambienti di sviluppo per supportare le attività automatiche. Anche il deployment del sistema dev'essere automatizzato in XP, mentre nel processo waterfall l'installazione del sistema viene fatta in un ultimo momento.

Integrazione continua significa che nell'ambiente di lavoro bisogna avere un tool che permetta quanto detto con un clic. Si deve scaricare allora il codice condiviso, si fa l'upload e il sistema in un momento di pausa deve lanciare una batteria di test di integrazione.

### Refactoring

Completato l’incremento, l’ideale sarebbe avere la release pronta. Tuttavia, poiché ogni pezzo è stato sviluppato da sotto-team diversi, bisogna unificare il lavoro: per qualche giorno, il team al completo analizza il codice con l’intento di capire se dei moduli hanno interazioni tra di loro o alcune cose potevano essere scritte meglio. L’importante è **evitare di introdurre nuove funzionalità**, in modo da poter **ri-eseguire gli stessi test scritti precedentemente** per verificare che il refactoring abbia migliorato il codice senza aver rotto alcuna funzionalità.

### Cliente in sede
Il cliente deve essere sempre reperibile, o alternativamente viene delegato un rappresentante per essere sempre presente e osservare quello che viene fatto. Tipicamente il modo di procedere della XP viene apprezzato anche dal punto di vista del committente in quanto può perfettamente vedere cosa avviene dietro le quinte della sua richiesta e viene coinvolto attivamente; si può quindi rendere conto che il lavoro ha un valore e può iniziare a sviluppare fiducia su quello che ha commissionato provando i vari progressi (ossia le release).

Senza questo tipo di approccio, in altri scenari, dopo che il cliente commissiona il progetto gli sviluppatori indicano il tempo che pensano di prendere e “finisce lì”: quest’ultimo deve aspettare senza poter vedere dei progressi se non chiedendo esplicitamente.

### Possesso del codice collettivo
Il **possesso del codice collettivo** è un'altra best practice che permette di analizzare le integrazioni aggiunte al sistema più recentemente.

Quando si sceglie il task da implementare, lo si fa senza la presenza di un manager che gestisce lo sviluppo. Da notare che il manager può essere presente, ma solo con un ruolo di alto livello, ad esempio:
- negoziare con il cliente le macro release;
- affittare le macchine e far installare il software per lo sviluppo.

Il manager quindi gestisce le attività che non riguardano il ciclo di esecuzione. Inoltre, il possesso del codice collettivo non riguarda solamente la possibilità di avere il codice, ma anche che la responsabilità del task è la responsabilità di un team. Non è detto che si riesca a completare il task in tempo, ma se un gruppo ha finito di lavorare ad un task prima, allora può subentrare. Se c'è un problema in un task, tutti sono responsabili e tutti possono agire.

### Settimana di 40 ore
Chiaramente la programmazione è un processo creativo. Si deve limitare il carico di lavoro del programmatore, in quanto solamente così si può minimizzare lo stress. Un programmatore stressato produce codice con errori.

### Usare gli standard del codice
Bisogna utilizzare notazioni standard per il codice. Poiché il nostro codice è la nostra documentazione, il codice dev'essere leggibile. Codice leggibile significa:
- indentazione corretta;
- camel casing;
- ogni identificatore ha un nome indicativo e suggestivo (ad esempio, chiamare una variabile "prezzo" probabilmente indica che la variabile memorizza il valore di un prodotto);
- evitare ripetizioni nella scrittura del codice.

## Riepilogo
Abbiamo visto diversi processi di sviluppo:
- Waterfall, rigidamente sequenziale e one-pass, con una sua alternativa con feedback;
- Evolutionary, che vede la trasformazione del prodotto da una versione iniziale ad una finale;
- Incrementale, che vede l'aggiunta di funzioni partendo da un'idea chiara del prodotto finale;
- Component Based, che prevede l'utilizzo di COTS, ossia Componenti Off-The-Shelf;
- Spiral, un framework iterativo per pianificare la gestione del rischio prima dello sviluppo del codice;
- Agile, una metodologia che agevola i cambiamenti utilizzando processi più leggeri, con XP.


# Refactoring
L'idea del refactoring è quella di riconoscere che è difficile ottenere fin dall'inizio una progettazione adeguata. I cambiamenti all'interno del codice permettono di incorporare nuovi requisiti più facilmente o requisiti che cambiano. Il refactoring fornisce tecniche per evolvere la progettazione in passi, riducendo il **debito tecnico**, ossia gli errori che abbiamo introdotto. Le strutture si trasformano in strutture più semplici e facili da capire.

## Estrazione del metodo
Un frammento di codice può essere estratto, facendo diventare quel frammento un metodo il cui nome spiega lo scopo del frammento. Ad esempio:
```java
public void printAll() {
    printBanner();
    System.out.println("Nome: ", nome);
    System.out.println("Cognome: ", cognome);
}
```
diventa:
```java
public void printAll() {
    printBanner();
    printInfos(nome, cognome);
}
public void printInfos(String nome, String cognome) {
    System.out.println("Nome: ", nome);
    System.out.println("Cognome: ", cognome);
}
```
I parametri locali diventano parametri del metodo creato. Inoltre, se il codice estratto ha variabili usate solo all'interno del codice estratto, si fanno diventare variabili temporanee del nuovo metodo.

Si riduce quindi la lunghezza dei metodi, rendendo i metodi più riutilizzabili. Inoltre, le funzionalità di alto livello sono più comprensibili, in quanto sembrano costituiti da una sequenza di commenti. Infine, se i metodi sono piccoli, eseguire overriding è più semplice.

## Sostituzione di variabili temporanee con metodi query
Le variabili temporanee tengono valori calcolati che non sono valori finali. Il calcolo intermedio potrebbe essere fatto non una volta, ma più volte. Se concettualmente è sempre lo stesso calcolo, il pezzo di elaborazione potrebbe essere incapsulato in un metodo a sé stante. Ad esempio:
```java
double prezzoBase = quantita * prezzo;
```
diventa:
```java
private double getPrezzoBase() { return quantita * prezzo; }
```

## Divisione della variabile temporanea
Se la variabile temporanea è indispensabile ed è assegnata più di una volta, ma non è una variabile assegnata in un loop, si può utilizzare una variabile separata per ciascun assegnamento.

Questo può essere utilizzato per evitare ambiguità ed errori. Si assegna quindi un nome indicativo ad ogni variabile che utilizziamo. La variabile, inoltre, è dichiarata come `final`, in modo tale che il compilatore controlli dove ci sono le successive assegnazioni alla variabile.

Dopo aver applicato questo refactoring possiamo applicare la sostituzione di variabili temporanee. 
