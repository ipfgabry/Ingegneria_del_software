Avere uno strumento per gestire la complessità software è importante. Ricordiamo che le tre sfide principali dell'ingegneria del software sono:
- gestire la complessità;
- gestire il cambiamento;
- gestire i difetti.

Il design Object Oriented ci permette di:
- **astrarre**, perché rappresentiamo simbolicamente le entità del sistema;
- **decomporre**, perché suddividiamo il sistema in sottosistemi, ad esempio in Java dividendo in package;
- **introdurre gerarchie**, tramite l'ereditarietà degli ADT.

## Modellazione
>[!note] Modellazione
>La modellazione consiste nella costruzione di un'astrazione della realtà.

Le app di navigazione ad esempio forniscono una buona astrazione di dove andiamo. Si tratta quindi di semplificare correttamente la realtà (che di per sé è molto **complessa**).
>[!note] Semplificazione corretta
>Una semplificazione è **corretta** quando i dettagli ignorati sono irrilevanti e i dettagli rilevanti sono rappresentati in maniera fedele alla realtà da rappresentare.

Prima di fare un modello, bisogna avere in chiaro alcuni elementi, tra cui **quale aspetto della realtà bisogna rappresentare**. All'interno del nostro modello, rappresentiamo questi aspetti in maniera astratta, passando da entità reali $R$ ad entità virtuali $M$. Bisogna inoltre tenere in considerazione le relazioni tra le entità che erano presenti nella realtà. Se esiste una relazione tra due oggetti reali $R_1,R_2$, denotata con $f_R$, allora devono esistere due entità virtuali $M_1,M_2$ in relazione tra loro, denotata con $f_M$.
### Sistemi, modelli e viste
>[!note] Modello
>Con **modello** si intende una **descrizione astratta** di un sottoinsieme di un sistema.

>[!note] Vista
>Con **vista** si intende un sottoinsieme degli aspetti del modello. Essa rappresenta aspetti specifici di questo.

>[!note] Notazione
>Con **notazione** si intende un insieme di **regole grafiche o testuali** per rappresentare viste.

Consideriamo ad esempio una macchina. Una macchina piccola che riproduce in scala una macchina è un **modello**. Un gioco basato sulla guida di auto è un altro modello che rappresenta un differente aspetto della macchina. 
Per costruire una macchina lego, che è un modello della macchina, abbiamo bisogno delle istruzioni. Tali istruzioni sono una **vista** del modello lego.
# UML (Unified Modeling Language)
Il modello **UML** è una notazione per la rappresentazione di sistemi object oriented. Il linguaggio UML nacque con l'intento di unire le **migliori prassi** per la progettazione di sistemi object oriented, definendo uno standard industriale unificato. Si tratta quindi di un modello **semi-grafico** e **semi-formale**, con una semantica precisa e un grande potere descrittivo, nonché con una collezione di diagrammi che permettono di descrivere i vari aspetti del modello realizzato. Esso consta di quattro viste, più una vista aggiuntiva che si sovrappone sulle altre, ossia la **vista dei casi utenti** (**use case view**). Le altre quattro viste sono:
- **logical view**, che descrive il **design** del sistema, in cui si esegue una sorta di fotografia statica delle entità che devono interagire nel funzionamento del sistema e delle loro relazioni con gli altri elementi. Le classi e le interfacce con le loro relazioni partecipano a questa vista, permettendo di analizzare le componenti dei moduli del sistema;
- **component view**, che descrive la struttura del design in termini di **modularità**, ossia come il sistema è diviso in moduli correlati tra loro. Permette quindi di dedurre l'architettura del sistema;
- **process view**, che permette di descrivere gli **aspetti dinamici** del sistema. I protagonisti sono gli **oggetti** e non più le classi, quindi gli elementi che a tempo di esecuzione sono presenti nel sistema. Si descrivono quindi le dinamiche del sistema;
- **deployment view**, che rappresenta come le componenti del sistema vanno mappate sulle parti fisiche che compongono l'ambiente in cui bisogna installare il software. Si descrivono quindi i requisiti delle componenti del sistema che deve supportare il software progettato.
## Diagrammi UML
Un diagramma può essere di uno dei due tipi:
- **statico**, o **strutturale**. A questa classe appartengono i seguenti diagrammi:
	- diagrammi delle classi;
	- diagrammi delle componenti;
	- diagrammi di struttura composita;
	- diagrammi di deployment;
	- diagrammi degli oggetti;
	- diagrammi dei package;
- **dinamico**, o **comportamentale**. A questa classe appartengono i seguenti diagrammi:
	- diagrammi delle attività;
	- diagrammi dei casi d'uso;
	- diagrammi di macchine a stati;
	- diagrammi di interazione, che a loro volta si distinguono in:
		- diagrammi di sequenza;
		- diagrammi di comunicazione;
		- diagrammi di interazione generale;
		- diagrammi di temporizzazione, dove vengono specificate dinamiche temporali con indicazioni quantitative. 

Tali diagrammi ci permettono di modellare il sistema in modi differenti:
- con la **modellazione funzionale** possiamo analizzare le funzioni del sistema. Utilizzeremo dei diagrammi dei casi d'uso;
- con la **modellazione a oggetti** analizziamo la struttura del sistema. Utilizzeremo dei diagrammi delle classi;
- con la **modellazione dinamica** si analizzano le dinamiche del sistema. Si utilizzano diagrammi di interazione.
## Diagramma Use Case
Il diagramma **use case**, o diagramma dei **casi d'uso**, è costituito da attori che permettono di modellare entità esterne che comunicano col sistema, che possono essere utenti, sistemi esterni (sotto questo aspetto, il sistema esterno non riguarda la nostra implementazione del sistema, pertanto è astratto con un attore generico, quindi i dettagli di tale sistema **non sono di nostro interesse**) o ambienti fisici.
All'interno del diagramma troviamo una box con dei "pallini", che rappresentano **funzionalità**. La box rappresenta i **confini del sistema** che stiamo sviluppando. All'interno del sistema ci possono essere funzionalità che derivano da altre funzionalità del sistema: in tal caso la funzionalità che ne estende un'altra viene rappresentata con una freccia verso la funzionalità estesa.
![[Pasted image 20250329111259.png]]
## Class diagram
I protagonisti di questa classe sono le classi. I simboli UML delle classi sono dei rettangoli che rappresentano le **classi** in cui:
- gli attributi sono rappresentati nella parte superiore del rettangolo;
- i metodi sono rappresentati nella parte inferiore del rettangolo.

Una rappresentazione tipica di una classe è la seguente:
![[Pasted image 20250329111932.png]]
In questo caso il simbolo $'+'$ indica che l'attributo o metodo è pubblico. Si utilizza $'-'$ per indicare che l'attributo o metodo è privato.
Da precisare che le classi non rappresentano obbligatoriamente classi da implementare, ma generalmente **entità** all'interno del nostro sistema.
Vi sono alcuni tipi di relazioni tra le classi:
- **generalizzazioni**, ossia relazioni di derivazione. Indica che un'entità è una generalizzazione di un'altra. Sono rappresentate con frecce a punta: la freccia parte dall'entità sottoclasse punta verso l'entità superclasse;
- **associazione**, che indica una corrispondenza generica. Su un legame di tipo associativo si può specificare una cardinalità (o **molteplicità**). Ad esempio, ad un entità "Dipartimento" possono essere associate una o più entità "Dipendente".  Per indicare un numero generico di entità in associazione, per la molteplicità utilizza la notazione $1...*$ o direttamente $*$. Tra le relazioni è possibile creare dei **vincoli**, ad esempio se ci sono più relazioni tra due o più entità coinvolte. Un vincolo può essere il vincolo **sottoinsieme**: un'associazione "manager", ad esempio, è sottoinsieme dell'associazione "membro", perché non vogliamo che un manager non sia membro del dipartimento;
- **dipendenza**, la forma di associazione più debole tra due entità. Significa semplicemente che un'entità dipende dall'esistenza di un'altra entità. Le dipendenze possono anche essere tra un metodo di un'entità e attributi di un'altra;
- **uses**, una relazione d'uso: un tipo usa un altro tipo. 
- **aggregazione**, in cui un'entità è definita come un aggregato di altre entità. Si rappresentano con dei rombi vuoti. Ad esempio, un record per un paziente è composto da un paziente e da un numero indefinito di consultazioni:
	![[Pasted image 20250326094138.png]]
- **composizione**, situazione più particolare dell'aggregazione in cui una classe esiste solo dentro istanze di un'altra classe, ossia una **inner class**, quindi dichiarata all'interno di un'altra classe o tale che il suo ciclo di vita dipende dalla classe in cui esiste. Si rappresentano con dei rombi pieni. Esistono anche **auto-composizioni**. Ad esempio, ad una persona possono corrispondere uno o più fratelli, che sono comunque persone, o un dipartimento è diviso in più sotto-dipartimenti;
- **interfaccia**, rappresentato con una linea con un cerchio vuoto. Rappresenta un'interfaccia per un'entità. Generalmente per rappresentare un'interfaccia in UML si utilizza la stessa metodologia che si usa per rappresentare le classi, ponendo sopra la parola "interfaccia". Il "lollipop" che utilizziamo allora indica che l'interfaccia viene **implementata**, quindi l'entità mette a disposizione dei suoi client le funzionalità dell'interfaccia. Il motivo per cui utilizziamo tale simbologia è per esprimere la possibilità di utilizzare una classe con l'interfaccia di un'altra:
	![[Pasted image 20250326094953.png]]
	In questo esempio, B utilizza l'interfaccia di A. Per rappresentare l'implementazione dell'interfaccia, si può anche utilizzare una freccia tratteggiata verso l'interfaccia, a differenza della freccia non tratteggiata, che rappresenta estensione:
	![[Pasted image 20250326095255.png]]
Un esempio di diagramma di classe è il seguente:
![[Pasted image 20250329111341.png]]
Nell'esempio vediamo `Company`, composizione (sebbene sia segnato come aggregazione, ma anche in aula abbiamo detto che è un errore) delle entità `Office` e `Department`. `Office` è una generalizzazione di `Headquarters`, sottoclasse di `Office`. Tra `Department` e `Office` l'associazione `Location` ha cardinalità molti a molti, indicata con $*\  *$. 
`Department` è dotata di una **auto-composizione**, che indica il fatto che un dipartimento possa essere costituito da più dipartimenti. Sono anche presenti due associazioni con `Person`:
- `member`, con cardinalità $1... \ *$ che indica che una persona è membro di uno o più dipartimenti;
- `manager`, con cardinalità $1$ dal lato di `Person`, che indica che la persona è manager di un solo dipartimento. Da notare che `manager` ha un vincolo `subset` con `member`: una persona è manager di un dipartimento solo se è anche membro di questo.

I metodi `getContactInformation()` e `getPersonalRecords()` presentano dipendenze con le classi `ContactInformation` e `PersonnelRecord`. `PersonnelRecord` è l'implementazione dell'interfaccia `ISecureInformation`.
## Diagramma delle attività
Supponendo di aver individuato una funzionalità del sistema attraverso uno use case, non è detto che si abbia chiarezza su come portare a termine tale attività. Un semplice metodo per modellare l'esecuzione della funzionalità è tramite il **diagramma delle attività**.
Il diagramma è caratterizzato da un pallino nero pieno per indicare l'inizio, un pallino nero con un cerchio esterno per indicare la fine e una serie di attività poste una dietro l'altra e concatenate con frecce. Se ci sono attività che devono essere eseguite in parallelo, si disegna una **barra di fork**,  che divide la freccia successiva in una o più frecce. Nel momento in cui queste attività si devono sincronizzare tra loro, ossia ci dev'essere un **join**, si riuniscono in una nuova barra e la freccia diventa una singola freccia nuovamente.
Se invece non si ha un parallelismo ma un'**alternativa del percorso di esecuzione**, si utilizza un rombo vuoto. Anche in questo caso ci sarà un **merge**, sempre indicato con un rombo.
Un'ulteriore complicazione si osserva nel caso in cui si organizzano le attività del diagramma in **colonne**, dette **swim lane**, in cui si individua il **responsabile** di certe attività. Il responsabile è individuato col nome della colonna.
Il diagramma delle attività ci permette di raccontare una funzionalità a livello dell'intero sistema. Risulta essere un procedimento che è ideale come inizio di un processo top-down per lo sviluppo del sistema. Ci aiuta quindi a realizzare e progettare uno use-case. 
Un altro aspetto importante riguarda la possibilità di **identificare istanze di oggetti** necessarie per lo svolgimento di una certa attività. Questi possono essere specificati con rettangoli con un nome dentro. Le istanze di oggetti, in particolare, non sono ancora con un tipo identificato.
![[Pasted image 20250329113545.png]]
## Collaboration Diagram
Lo scopo del **diagramma di collaborazione** è quello di descrivere le interazioni tra oggetti. Il flusso dei messaggi è indicato da **frecce** accanto alle associazioni tra istanze che partecipano all'interazione. 
Questo diagramma permette di identificare quale interazione viene svolta, quindi quale **metodo** o **messaggio** viene inviato ad un altro oggetto. Si identifica con un **numero sequenziale** anche un ordine temporale con cui avvengono i messaggi. 
Il diagramma mette in evidenza quali oggetti sono coinvolti e quali interagiscono tra di loro. Ogni interazione tuttavia provocherà una serie di interazioni differenti tra gli elementi del sistema. Pertanto si avranno tanti diagrammi, ognuno dei quali definisce una serie di interazione tra i vari elementi del sistema.
![[Pasted image 20250329113633.png]]
## Diagramma di sequenza
In questo tipo di diagramma si mette in evidenza la sequenza temporale delle azioni. Questo tipo di diagrammi può essere di due modi:
- il **diagramma a scaletta**, più comune. Un software a oggetti utilizza comunque una esecuzione sequenziale di chiamate e interazioni tra oggetti. In questo diagramma si rappresenta allora un omino, che indica che ad iniziare un'attività è qualcuno o qualcosa che prende iniziativa e fa scattare il primo step dell'interazione. Ci sono poi una serie di entità coinvolte, rappresentato da rettangoli. Il rettangolo viene presentato prima perché significa che si ha un'**istanza di oggetto**. Una linea tratteggiata individua una **timeline**; alla fine della timeline, una 'x' indica che l'**istanza viene distrutta**. Nella JVM se un oggetto rimane senza riferimenti, il garbage collector de-alloca automaticamente questi oggetti. Un rettangolo pieno presente nella timeline indica che l'istanza di oggetto è in esecuzione. Il diagramma a scaletta è ideale per indicare logiche di funzionamento **distribuite**, in cui ogni oggetto partecipa all'implementazione del software;
	![[Pasted image 20250329114137.png]]
	come si vede dall'immagine soprastante, allo studente `s` viene assegnata la creazione dell'esame, seguendo un sistema di chiamate a cascata (quindi chiamate distribuite tra gli oggetti del sistema);
- il **diagramma a fork**, ideale per indicare logiche **centralizzate**: un attore centrale si occupa di scambiare messaggi con le istanze di oggetto presenti nel sistema.
	![[Pasted image 20250329114237.png]]
	nella frazione mostrata, la classe `MainEsami` si occupa di inizializzare lo studente `s`.

Può capitare che una certa interazione si ripeta più volte. Allora si racchiude uno scambio di messaggi in una box e si aggiunge un'etichetta **loop**:
![[Pasted image 20250329114316.png]]
Nella frazione soprastante, l'etichetta loop sottolinea che da un altro oggetto (a destra un oggetto di tipo `Esami`) bisogna recuperare le informazioni "matricola" e "voto". Come si deduce dal nome della procedura chiamate (`printAll()`), il processo viene ripetuto per ogni esame.
## Diagramma degli stati
Il **diagramma degli stati** permette la rappresentazione del ciclo di vita delle entità del sistema. Supponiamo di avere una classe rappresentante un ADT con dei dati interni che rappresentano uno stato che cambia nel tempo. Vogliamo rappresentare il ciclo di vita dell'entità. 
Un cerchio nero rappresenta lo stato iniziale dell'oggetto. Un altro cerchio nero con un contorno indica lo stato finale. Indica esplicitamente che si esce dallo stato. Frecce etichettate descrivono eventi che portano a cambiamenti di stato. Dei rettangoli con angoli arrotondati rappresentano gli stati dell'entità.
![[Pasted image 20250329114947.png]]
Le transizioni da uno stato ad un altro non devono solamente avere un nome, ma possono avere una **guardia**, ossia una **condizione** che se non viene verificata impedisce che la transizione avvenga. Possono anche avere un'**azione abbinata** che produce un certo risultato in virtù del cambiamento di stato. Si utilizza la notazione: $$\text{evento [guardia] / azione}$$All'interno dello stato si può definire un insieme di azioni di ingresso e di uscita e azioni che caratterizzano quello stato. 
![[Pasted image 20250329115029.png]]
Gli stati, inoltre, possono essere **composti e paralleli**. Questo dà la possibilità di descrivere uno stato come un sotto-diagramma degli stati, permettendo una modellazione più precisa dello stato di un oggetto. Possono esistere anche degli stati **concorrenti** e utilizzare quindi i meccanismi di fork e join:
![[Pasted image 20250326105548.png]]
In questo caso alla fine della transizione da entrambi gli stati presenti nello stato "Welcome" si giunge allo stato finale.