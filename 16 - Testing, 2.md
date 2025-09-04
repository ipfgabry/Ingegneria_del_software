# Tipologie di doppioni di test
Parliamo di **test doubles**, ossia di **doppioni di test** o **controfigure**, per indicare le componenti che sono simulate all'interno del sistema:
- **Dummy objects**. Gli oggetti **dummy** sono passati come oggetti per riempire i parametri di funzione, ma non sono realmente usati.
- **Oggetti finti**. Gli **oggetti finti** hanno un'implementazione funzionante, ma servono solo per implementare un'interfaccia necessaria.
- **Stubs**. Gli stub sono definiti in modo tale che i metodi di uno stub restituiscono il valore di ritorno atteso. Non rispondono alla reale funzionalità richiesta, ma semplicemente al valore che è atteso dalla computazione del modulo software analizzato.
- **Spie**. Gli oggetti spie sono oggetti stub che raccolgono delle informazioni basate su come questi vengono chiamati. Ad esempio, un servizio email potrebbe registrare quanti messaggi sono stati inviati.
- **Mock objects**. Gli oggetti mock sono oggetti che sono programmati in modo tale da sapere quante volte e in quale ordine le funzioni devono essere chiamate durante i test. 

## Mock e stub
Gli stub servono a fornire dei dati di ritorno alle chiamate della classe testata. Si decide l'esito dei test con delle asserzioni che verificano lo stato della classe testata. 

I mock invece sono oggetti finti che controllano il comportamento delle classi sotto test. Le interazioni della classe sotto test sono tracciate con il mock, che analizza il numero, l'ordine e i valori dei parametri delle chiamate ricevute durante il test. Le asserzioni esaminano lo stato del mock, e non la classe testata, che invece comunica con la classe mock al fine di fornire le statistiche per l'oggetto mock.

Il tipo di testing in entrambi i casi è un **behavioural testing**.

## Behavioural Driven Development
Il criterio di accettazione per validare le funzionalità sotto un test è uno **scenario** che segue la struttura:
- **given**, che indica il contesto iniziale dello scenario in una o più clausole. Si specifica cosa dev'essere restituito dai metodi mock quando vengono invocati;
- **when**, che indica l'evento che attiva lo scenario. La classe testata viene usata e usa in cascata il mock object;
- **then**, che indica il risultato atteso analizzando asserzioni sullo stato delle interazioni. 

## Mockito
La libreria Mockito permette di semplificare la scrittura dei test tramite Mock Objects. Utilizza una sintassi compatta:
```java
@Mock
Collaborator mockObj;
//in alternativa...
Collaborator mockObj = mock(Collaborator.class);
```
Mockito permette di:
- indicare direttamente durante l'esecuzione del test quale valore restituire quando un metodo del mock è invocato;
- verificare con quali input un metodo del mock sia stato chiamato durante l'esecuzione del test.

### Esempio

```java
Seller seller = mock(Seller.class);
Shop shop = new Shop(seller);

public void shouldBuyBread() {
    //given
    given(seller.askForBread()).willReturn(new Bread());
    //when
    Goods goods = shop.buyBread();
    //then
    assertThat(goods, containBread());
}
```
Il mock object `seller` viene impostato in modo tale che quando viene invocato il metodo `askForBread()` si restituisca una nuova istanza di `Bread`. L'evento che attiva lo scenario è il metodo `buyBread()` di `shop`. Si verifica infine se `goods` soddisfa la condizione implementata all'interno di `containBread()`.

Si prevede anche l'utilizzo di un metodo `then()`, che valuta se un metodo viene effettivamente chiamato con quei valori:

```java
given(summer.sum(a,b)).willReturn(5);

int res = summer.sumTwice(a,b);

then(summer).should().sum(a, b); // verifica che il metodo viene chiamato con quei valori
assertThat(res).isEqualTo(10);
```
Scrivere i test prima della codifica del software è un approccio migliore. Per verificare invece quante volte è stato chiamato un metodo, si utilizza come parametro di `should()` il metodo `times(n)`, dove con `n` si indica il numero di volte in cui il metodo dev'essere invocato. Esistono inoltre:
- `shouldHaveNoMoreInteractions(obj)`, che specifica che non bisogna avere più nessun'altra chiamata;
- `shouldHaveZeroInteractions(obj)`, che specifica che non ci devono essere chiamate a metodi di un oggetto.

## Test parametrici
Poiché per eseguire i test bisogna campionare il dominio di input, vorremmo automatizzare il test su elenchi di dati molto ampi. Si definisce allora un set di dati, dove ogni dato è inserito in un array. Il set di dati statico viene annotato con `@Parameters`:
```java
@Parameters
public static Collection<Integer[]> getParam() {
    return Arrays.asList(new Integer[][] {
        {1, 1, 2},
        {3, 2, 5},
        {4, 3, 7}
    });
}
```
I parametri vengono passati ad ogni metodo di classe di test. I test prevedono, in questo caso, tre parametri.

## White-box testing e Black-box testing
Le principali differenze tra white box e black box testing sono illustrate nella tabella sottostante.

Black-Box | White Box|
--|--|
I test dipendono solo dalle specifiche | I test dipendono dall'implementazione e dalle specifiche
I test possono essere riusati se il codice viene aggiornato | I test sono invalidati nel momento in cui modifichiamo il codice
I test possono essere sviluppati prima che il codice viene scritto | I test possono essere sviluppati solamente dopo la scrittura del codice
I test non richiedono che tutto il codice venga eseguito. Possono tuttavia non controllare gli **errori di commissione** | I test non assicurano che il codice implementi tutte le specifiche. I test non controllano quindi gli **errori di omissione** (manca la funzionalità nel codice)
Alcuni strumenti forniscono misurazioni di copertura di test black box | Molti strumenti forniscono misurazioni di copertura di test white-box

Gli strumenti si completano, pertanto si utilizzano entrambe le tipologie di test, partendo dai test black box e procedendo con test white box. 
### Black Box Testing
Si inizia solitamente con test black box. Le tecniche più comuni per fare test black box sono tre:
- **Equivalence Partitioning**. Si partiziona il dominio di ingresso, e si rappresenta ogni partizione con un singolo test rappresentativo della partizione;
- **Boundary Values Analysis**. In questo modo si verifica la correttezza dell'operazione ai confini degli intervalli del dominio;
- **Combinatorial Interaction Testing**. Se le specifiche prevedono processamenti differenti in virtù dell'input, si eseguono test con diverse combinazioni di input.
### White box testing
Il white box testing misura la copertura di statement e di branch per i test black box, permettendo il testing di tutte le parti del software da testare e migliorando quindi la fase di validazione. Si utilizzano le seguenti tecniche: 
- **Statement Coverage**. Si utilizza lo statement coverage per verificare che il 100% degli statement viene eseguito;
- **Branch Coverage**. Si utilizza il branch coverage per verificare che il 100% di tutti i branch viene attraversato (in un flowchart, questo corrisponde a percorrere tutti gli archi);
- **All Paths**. Si controllano tutti i percorsi di esecuzione;
- **Definition-Use Pairs**. Si testa il codice nel caso in cui esso contenga dei pattern per l'uso di dati;
- **Condition Coverage**. Si occupa di coprire le decisioni all'interno del codice.

### Fault model (definizione generica)
Un **fault model** è un modello di qualcosa che può andare storto. Per ogni tipo di test, esiste un fault model che permette di prevedere le conseguenze di un determinato fault.
## Equivalence partition
Identifichiamo il dominio di ingresso e lo dividiamo in partizioni. Questo potrebbe richiedere come rappresentare i nostri parametri. Se i parametri sono di tipo base, possiamo utilizzare dei range comuni, quali valore minimo e valore massimo. Se tuttavia abbiamo tipi che non presentano un ordine naturale, dobbiamo trattare ogni singolo valore come un range separato. Oppure si potrebbero avere tipi come tipo stringa, e in generale il partizionamento dipende dalla specifica: bisogna infatti capire come la stringa utilizzata influenza il risultato dell'elaborazione. Nel caso più comune, se aspettiamo una stringa che rappresenta un qualunque elemento, distinguiamo stringhe ben formate da stringhe malformate e dalla stringa nulla. 

Come dominio di input non abbiamo solamente i parametri espliciti del metodo invocato, ma bisogna anche far riferimento agli attributi statici.

Una volta aver identificato il dominio e averlo diviso, si sceglie un valore per partizione. Ogni partizione per ogni input e output è un **oggetto di test coverage**. Solitamente si fornisce un identificatore unico, utilizzando come prefisso EP. Si individuano inoltre quali oggetti di test coverage prevedono fallimenti nel sistema. Se il test tuttavia fallisce e al suo interno quello che aveva stimolato erano due possibili esecuzioni in cui il sistema poteva fallire, non si può individuare correttamente quale test case ha fatto fallire il sistema. Pertanto ogni test coverage item che contiene input di errore viene testato separatamente.

Si scrivono quindi tanti test case quanti sono i test item in virtù delle specifiche del sistema e dei metodi da invocare. Il numero di test scritti dev'essere tale che esiste almeno un test per EP: in questo caso diremo che la copertura dei test è pari al 100%. Alcuni test inoltre possono essere ridondanti e pertanto possono essere eliminati. 

### Esempio
Supponiamo di voler testare una funzione `Status giveDiscount(long bonusPoints, boolean goldCustomer)` con le seguenti specifiche:\
![alt text](<../!Images/16 - Testing, 2/image.png>)\
Per `bonusPoints` possiamo allora ottenere le partizioni `MIN_VALUE, 0`, `1, 80`, `81, 120`, `120, MAX_VALUE`. Per `goldCustomer` e il valore di ritorno (che è un'enumerazione) utilizziamo semplicemente i valori discreti possibili.\
Ogni oggetto di test coverage è identificato da una scelta di valore per partizione:\
![alt text](<../!Images/16 - Testing, 2/image-5.png>)\
Eseguiamo allora dei test combinando i vari TCI, **cercando di minimizzare il numero di test da scrivere**. Nel nostro caso possiamo testare le funzionalità con quattro test:\
![alt text](<../!Images/16 - Testing, 2/image-1.png>)\
Nel processo cerchiamo anche di evitare la scrittura di test duplicati, ossia test che coprono gli stessi TCI.

### Fault model di EP
Il fault model dell'equivalence partitioning modella range di valori che non vengono processati correttamente. Testando almeno un valore da ogni partizione di equivalenza, in modo tale che **ogni valore dello stesso range sia processato nella stessa maniera**, si possono trovare i fault di questo tipo. 

## Boundary Values Analysis
Il test EP non riesce tuttavia a individuare errori relativi a valori che si trovano **agli estremi di una partizione**. Eseguiamo allora dei test **Boundary Value Analysis**.

Nell'esempio illustrato precedentemente, i valori 0, 1, 80, 81, 120, 121, `MIN_VALUE` e `MAX_VALUE` devono essere testati. Allora introduciamo altri TCI, etichettati con `BV`. Otteniamo allora i seguenti test:\
![alt text](<../!Images/16 - Testing, 2/image-2.png>)\
Statisticamente si mostra come la BVA sia in grado di trovare più errori rispetto alla EP. Poiché inoltre per ogni range si può individuare un minimo ed un massimo, la BVA fa testing di due valori aggiuntivi per ogni partizione. 

### Combinatorial Testing
BVA non è in grado di esplorare decisioni associate a combinazioni di input. Si utilizza allora il **Combinatorial Testing**.\
Il Combinatorial Testing cerca di analizzare tutte le combinazioni di input. Tuttavia, questo può portare ad un grande numero di combinazioni. Supponendo di avere 20 variabili, ognuna con 10 valori ciascuno possibili, si ha un numero totale di test pari a $10^{20}$.\
Invece di provare tutte le possibili combinazioni dei parametri, possiamo ridurre l'ampiezza delle combinazioni, ad esempio decidendo di fare test di tutte le coppie di parametri. Questo perché è stato osservato che in quasi tutte le applicazioni il 60% dei fault è attivato dall'interazione di coppie di parametri. La percentuale si avvicina al 100% quando interagiamo con 6 parametri. Pertanto si può utilizzare un criterio di testing che prende il nome di **pairwise testing**, ossia si generano suite di test per coprire le combinazioni di coppie di parametri. Il ragionamento del pairwise testing permette di semplificare il numero di test. Ad esempio, avendo 5 parametri che possono avere rispettivamente 3,2,2,2,3 valori distinti, si ha un numero totale di test pari a $3^2\cdot 2^3 = 72$. Con il pairwise testing, possiamo ridurre questo numero a 10. Si dice che il pairwise testing è un tipo speciale di **$t$-way coverage testing**, in cui $t=2$. Quindi per quanto detto precedentemente, basta scegliere $t \in [2,6]$ per ottenere un numero sufficiente di test.

Per il testing combinatorio, avendo una forza $t$ di coverage testing, $v$ valori possibili per parametro e $k$ parametri è necessario costruire un **covering array** di dimensione $N$, che non è altro che una matrice $N\times k$ con simboli provenienti da un alfabeto di $v$ simboli tale che in ogni sottomatrice $N\times t$ si ha che una qualsiasi combinazione di $t$ elementi dell'alfabeto di $v$ simboli è coperto almeno una volta. Ad esempio, supponiamo di voler testare 4 parametri binari con un test di forza 2. In teoria avremmo un numero di test pari a $2^4 = 16$ test da fare. Tuttavia, visto che ci interessano le coppie di parametri, basta considerare le coppie binarie 00, 01, 10, 11. Invece di considerare il set `0000, 0001, 0010, 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101, 1110, 1111`, basta considerare il set `0000, 0111, 1011, 1101, 1110`. Passiamo quindi da 16 test a 5. In questo caso cerchiamo un valore $N$ tale che $N$ sia minimo, per ottenere quindi un covering array minimo. Nell'esempio proposto, quindi, $N = 5$ è minimo e peremtte di costruire la tabella $5 \times 4$:

Test | A | B | C | D
--|--|--|--|--|
1| 0| 0| 0| 0
2 | 0|1|1|1
3| 1|0|1|1
4|1|1|0|1
5|1|1|1|0

Notiamo come prendendo una qualsiasi sottomatrice $5\times 2$ tute le combinazioni di due elementi dell'alfabeto binario $\Sigma = \set{0,1}$ sono coperte.

Il problema risiede nel fatto che il calcolo di un covering array minimo è **NP-completo**, ossia non si ha un algoritmo che in tempo polinomiale generi il covering array, ma si ha un algoritmo che in tempo polinomiale verifica se il covering array è minimale.

### Statement Coverage
Lo statement coverage permette di verificare se ogni riga di codice **eseguibile** viene eseguita durante i test, verificando intanto che l'output è corretto. Ogni statement eseguibile nel codice è un TCI. 

Si costruisce un grafo che descrive tutti i possibili flussi di esecuzione dell'algoritmo da testare. Si controlla manualmente tramite i TCI già coperti quali sono le parti eseguite.

### Grafo di controllo di flusso
Il grafo di controllo di flusso è un grafo in cui ogni percorso da un estremo ad un altro rappresenta il flusso di esecuzione di un metodo o di un programma. Il grafo rappresenta un modello astratto dell'algoritmo in termini di flusso di esecuzione, e permette il calcolo della **complessità ciclomatica**. Ogni `if` è una biforcazione, mentre ogni `loop` è un ciclo nel grafo. 

La complessità ciclomatica indica quanto è complesso un algoritmo. La complicazione cresce quando cresce il numero di branch all'interno dell'algoritmo. Questo è indipendente dal fatto che si distinguono `if` e loop. La complessità ciclomatica si calcola tramite il calcolo numero di archi - numero di nodi + 2, ed **è il numero minimo di test, ossia di percorsi distinti, per percorrere il grafo nella sua interezza**. Ma questo **non è il numero totale di percorsi di flussi**, ossia il numero di percorsi distinti.\
![alt text](<../!Images/16 - Testing, 2/image-4.png>)

Per la costruzione di un grafo di controllo di flusso, si può utilizzare **JaCoCo** (Java Code Coverage). JaCoCo fornisce un output in XML o HTML, e permette di identificare quali istruzioni sono state coperte o meno.

### Fault model di Statement Coverage
Il fault model che è dietro lo statement coverage è l'individuazione degli errori legati al comportamento del sistema in generale. Inoltre, questi test sono in grado di notare la presenza di **codice morto** o di **statement non utilizzati**.\
Questa idea tuttavia non è la più potente e alcuni errori possono restare in quanto i test nel complesso coprono gli statement, ma non hanno come obiettivo l'esecuzione completa di un percorso completo di esecuzione di un algoritmo. Per testare tutti i flussi di esecuzione, si utilizza il **branch coverage**.

### Branch coverage
Il problema dello Statement Coverage risiede, oltre nel capire quali input utilizzare, nel capire come si può testare il codice in contesti inusuali. Inoltre, **non si copre il caso in cui si ha un `else` nullo**:
```java
if(number < 3)
    number++
```
Lo statement coverage non copre un test case per valori che sono maggiori o uguali a 3. Per risolvere questo problema, si utilizza il **branch coverage**.\
I branch che non sono stati coperti dai precedenti test potrebbero contenere dei fault. Allora si utilizza il branch coverage per coprire i casi rimanenti, analizzando i flussi di esecuzione e facendo si che ogni branch sia eseguito.

Ad esempio, dato lo snippet:
```java
if(x && y)
    z = 10;
else
    z = 20;
```
Abbiamo due **branch decisionali**:
- un branch se la condizione è verificata;
- un branch se la condizione non è verificata.

In totale abbiamo **4 branch basati sulle condizioni**:
- se la condizione `x` è vera si passa a `y`;
- se la condizione `x` è falsa si passa alla riga 4;
- se la condizione `y` è vera si passa alla riga 2;
- se la condizione `y` è falsa si passa alla riga 4.

Il fault model in questo caso riguarda la copertura di branch non coperti da precedenti test che potrebbero contenere un errore. Come per lo statement coverage, in questo caso i fault si verificano in condizioni non usuali.
### All-Path coverage
La forma più potente di white-box testing è la **All-path coverage**.\
Per applicare l'all path coverage necessitiamo del grafo di controllo di flusso. Si enumerano **tutte le combinazioni** all'interno del grafo. Passare ad un determinato percorso significa passare a determinate condizioni in cui i valori coinvolti potrebbero avere assunto assegnamenti non compatibili. Quindi **non è detto che tutti i percorsi possano essere eseguiti**. Questo controllo tuttavia va fatto manualmente.

Il fault model del coverage all-path riguarda sequenze di operazioni (rappresentate come percorsi nel codice) che non producono il risultato corretto. Questi fault sono associati a strutture di codice dove la funzionalità scorretta è eseguita per una situazione specifica. Si trovano quindi errori funzionali che si manifestano solo con l'esecuzione di una sequenza più lunga di un singolo branch.

L'all-path coverage tuttavia:
- è un processo lungo;
- non valuta tutte le condizioni booleane in ogni decisione;
- **non esplora fault dovuti a processamenti errati di dati o errori non dipendenti dal codice**.

### Dataflow coverage /Definition-Use pairs, Condition and Decision coverage
Varianti del branch coverage sono:
- **dataflow coverage** (o **definition-use pairs**): si coprono tutti i percorsi tra la definizione di un dato e il suo primo utilizzo (per questo si chiama definition-use: si considerano le coppie definizione del dato - primo utilizzo) Si tratta di un criterio di fault che cerca errori nel modo in cui si utilizzano i dati. Non si tratta quindi di un'invocazione sbagliata, ma è sbagliato il modo in cui si utilizza un valore (i criteri visti precedentemente non analizzano i valori). Si tratta di un test molto forte ma che potrebbe portare ad un numero di test molto grande;
- **condition coverage** (**CC**): si esegue un test per ogni condizione booleana. Non garantisce che la decisione è vera o falsa, ma solo la condizione booleana. Inoltre può essere difficile stabilire i parametri di input richiesti;
- **decision coverage** (**DC**): si esegue un test per ogni decisione. Equivale ad un branch coverage se la misurazione della copertura utilizza le decisioni come branch;
- **decision/condition coverage** (**DCC**): i test sono generati per ogni decisione e per ogni condizione, senza testare tutte le possibili combinazioni delle condizioni;
- **multiple condition coverage** (**MCC**): i test sono generati per testare tutte le possibili combinazioni di test booleani. Si hanno tuttavia molti TCI, in quanto per $n$ condizioni booleane si hanno $2^n$ TCI;
- **modified condition/decision coverage** (**MCDC**): il numero di test di MCC viene ridotto per considerare solo le combinazioni di condizioni booleane per causare un effetto visibile sull'output. Per capire meglio consideriamo lo snippet sottostante:
  ```java
    int x = 100;
    if ( (a > 10) || (b == 0) ) then x++;
    return x;
  ```
  I test per lo snippet devono considerare le condizioni per i casi `true` e `false`:
  - `a > 10`;
  - `b == 0`.
  
  I test inoltre devono considerare la decisione `( (a > 10) || (b == 0) )` per i casi `true` e `false`. Infine, i test devono analizzare l'effetto sul valore di output nel cambiamento del valore delle condizioni booleane.

## Ranking dei test
Il ranking dei test può essere riassunto con il grafico sottostante:\
![alt text](<../!Images/16 - Testing, 2/image-3.png>)