# Validazione e verifica
Nello sviluppo di software non si ha solo una complessità dovuta alla difficoltà del problema, ma anche una complessità **intrinseca** allo sviluppo. Possiamo distinguere due "tipi" di complessità:
- la complessità **accidentale**, legata principalmente all'implementazione, pertanto può essere rimossa cambiando il codice o il linguaggio impiegato;
- la complensità **intrinseca**. Questa rimane anche se tutte le complessità accidentali sono state rimosse. Può essere controllata sviluppando un comportamento a run-time più prevedibile.

## Complessità intrinseca
Per quanto si possa essere chiari nello scrivere a tempo di compilazione, dietro la scrittura chiara si nasconde un insieme di comportamenti a tempo di esecuzione che sono dinamici. Il solo fatto di utilizzare astrazioni permette di rendere chiaro il codice in maniera concettuale, ma ciò che poi effettivamente accade passo dopo passo, ossia il **comportamento a tempo di esecuzione**, non è visibile.

Ad esempio la seguente riga di codice è concettualmente corretta:
```java
do j++ while (a[j] < pivot)
```
Ma **non sappiamo se il ciclo termina**. La terminazione del ciclo infatti dipende dalla presenza di un elemento `a[j]` che sia più grande di `pivot`.

Facciamo quindi fronte ad un software in cui sono presenti dei difetti. Tecnicamente distinguiamo dal punto di vista lessicale l'errore dalla sua manifestazione. Il **fault** è la parte del programma che causa un errore, ed è quindi il pezzo di istruzione che causa un comportamento sbagliato del programma. Un fault non è necessariamente qualcosa che sintatticamente o semanticamente è individuabile a tempo di compilazione: potrebbe essere una scrittura sensata sia sintatticamente sia semanticamente, ma l'errore potrebbe anche essere più difficile da debuggare poiché non si riesce ad associare la fault con la **failure**. Sinonimi di fault sono "bug", "difetto" e "anomalia".\
Chiamiamo **failure** un comportamento anomalo a tempo di esecuzione.

## Prove di correttezza
Possiamo eseguire due prove per verificare la correttezza del software: la **validazione** (o **testing**) e la **verifica**.

### Validazione
Nella fase di validazione si eseguono dei test, concentrandosi su che cosa il software ha prodotto come risultato. Si verifica il "cosa" del software (ossia, il prodotto sviluppato è corretto?), ricevendo un'indicazione sulla correttezza del risultato. La validazione analizza il punto di vista dell'utilizzatore del prodotto. I test inoltre possono essere di due tipi:
- test **a scatola nera**, ossia dove i test sono basati su input e output;
- test **a scatola bianca**, ossia test che si basano sulla conoscenza del codice.

### Verifica (analisi formale)
L'analisi formale è una verifica del come viene sviluppato il prodotto. Si controllano quindi se i meccanismi interni sono corretti per lo scopo. La verifica è mollto più efficace del testing: se verifichiamo la correttezza dei meccanismi interni, la risposta valida è molto più consistente come informazione, in quanto indica che i meccanismi interni funzionano e non possono causare comportamenti sbagliati. Per dare questa risposta, non si eseguono semplicemente dei test, ma si guarda l'implementazione.

Poiché i software tendono ad essere esponenzialmente complessi e la validazione è più semplice, si esegue la verifica solo per funzionalità e porzioni di software invece dell'intero software. Alcuni strumenti inoltre permettono l'analisi di proprietà (ad esempio, la terminazione di un ciclo) tramite l'utilizzo di linguaggi formali: si trasforma il codice in una serie di predicati e si verifica che la proprietà sia una tautologia. 

## Testing
### Scelta dei test
La fase di testing è divisa in quattro fasi:
1. scelta del software da testare e del tipo di test;
2. impostazione dell'ambiente di testing, in modo tale che il codice possa essere eseguito ripetutamente;
3. selezione di un set di input basandosi su criteri;
4. valutare gli output ottenuti.

Per capire se un set di input è buono per un test, si parte dalla **specifica dei requisiti**. I test inoltre devono essere scritti in maniera corretta, facendo sì che non si sovrappongano tra di loro, in modo tale da evitare di scrivere test non necessari. Se inoltre il principio di singola responsabilità è rispettato, è possibile scrivere dei test che validano le funzionalità di una classe.

La funzionalità analizzata dai test può essere una funzionalità singola o una funzionalità macroscopica. Individuare la funzionalità è il primo passo; per testarla correttamente, il problema principale è dare l'input, selezionando correttamente partendo da un insieme universo di input possibili. Il test inoltre, come visto precedentemente, non fornisce una risposta definitiva sulla correttezza del software: per questo motivo è necessario eseguire un numero sufficiente di test.\
Il problema non riguarda solamente l'input, ma anche la deduzione dell'output. Se l'output con cui confrontarsi è complesso, vi è il problema dell'**oracolo**. Fintanto che si eseguono test di pezzi di implementazione, la specifica del software permette di dedurre il risultato atteso. Man mano che si passa dal singolo metodo all'intera classe o ad un sottosistema, fino ad arrivare all'intero sistema, ci si basa sempre sulla specifica, ma ci si allontana dal dettaglio implementativo.\
Nell'esecuzione del testing inoltre, per verificare se un modulo software è corretto è necessario **isolare** il modulo. Isolare non significa tuttavia portare fuori il modulo dal suo sistema, ma inserire il modulo in un **environment** in modo tale che venga "simulata" l'interazione con l'ambiente.

### Selezione dei campioni
Il testing può essere visto come una sorta di **campionamento**. Il sistema viene analizzato in virtù di campioni scelti. Il campionamento degli input tuttavia dev'essere tale da rappresentare in maniera corretta l'insieme universo degli input. In virtù del campionamento si può dedurre se il sistema è affidabile e corretto. Esistono diversi **criteri di campionamento**, ognuno dei quali fornisce una **metrica di copertura**: essa ci permetterà di capire quanti sono i test da svolgere per poter validare il software. 

A questo punto si può capire la differenza tra i test black box e white box:
- se l'implementazione del sistema non è visibile (black box), i test black box sono costruiti in modo tale da associare ad un input un output. I criteri di copertura per metriche per test di tipo black box guardano solamente gli input, ragionando su come dividere il dominio dei valori di input in modo tale da ottenere un insieme di campioni rappresentativo della popolazione;
- metriche per test di tipo white box campionano nello spazio delle esecuzioni. Il codice rappresenta $n$ flussi di comportamento, pertanto per essere sicuro che il programma lavori bene bisognerebbe provare tutti questi flussi. Si campiona in uno spazio noto come **spazio dei comportamenti**.

### Terminologia
> [!NOTE] Unit Testing 
> Prende il nome di **unit testing** il testing eseguito su singole funzioni o singole classi.

> [!NOTE] Regression Testing
> Prende il nome di **regression testing** l'esecuzione di test già eseguiti in precedenza per verificare che, dopo un cambiamento, tutto funzioni correttamente.

> [!NOTE] Test suite
> Prende il nome di **test suite** un insieme di test di input.

> [!NOTE] System Verification Group
> Il **System Verification Group** è un team dedicato al testing.

Il test può essere eseguito a più livelli:
- test di **unità**, che ha per oggetto le parti atomiche del sistema;
- test di **integrazione**, che testano le interazioni di funzionalità atomiche per eseguire funzionalità di più alto livello. Le interazioni possono riguardare anche singoli moduli;
- test **funzionali**, che verificano le funzionalità previste dalle specifiche che deve rispettare il software. Si analizza quindi il design del sistema;
- test di **sistema**. Spesso il test funzionale è incluso nel test di sistema, in quanto l'oggetto di testing è l'intero sistema in entrambi i casi. Nel test di sistema non si guardano solo le funzionalità, ma anche aspetti tecnici che nel test funzionale non sono presenti;
- test di **accettazione**, che verificano il sistema in scenari di utilizzo. 

Gli esiti dei test sono di quattro tipi:
- positivo;
- negativo;
- falso positivo;
- falso negativo.

Gli ultimi due esiti riguardano:
- test che **dovevano fallire** ma che hanno avuto esito positivo. In questo caso si **sa già che il test fallisce**, ma il test dà esito positivo;
- test che **dovevano avere successo** ma che hanno avuto esito negativo. 

Tipicamente si tende ad utilizzare dei test white box solamente quando siamo vicini al codice (ossia test di unità). Per test di integrazione o di livello superiore si eseguono dei test di tipo black box. Solitamente i test di validazione sono di tipo black box, mentre i test di verifica sono white box (si possono anche utilizzare test black box in questo caso). Si utilizzanno anche metodi agili per testare il codice durante la scrittura di questo.\
Esistono inoltre strumenti che permettono la **generazione automatica di test** e l'esecuzione automatica di questi. Per Java un software di test utilizzato è la **JUnit**.\
Poiché non abbiamo bisogno obbligatoriamente del codice per scrivere i test, possiamo scrivere i test **prima di scrivere il codice**: questo permette di guidare la programmazione tramite i test. Si scrivono prima i test in base alla specifica, e man mano che si implementano le funzionalità si modifica il codice in virtù degli esiti. Questo approccio è un approccio intelligente e prende il nome di **Test Driven Development** (**TDD**).\
Si può anche specificare il comportamento del sistema in termini di interazione e guidare la produzione di codice in virtù del comportamento del software da produrre. Questo approccio prende il nome di **Behaviour Driven Development** (**BDD**).  

### Environment
L'utilizzo di un environment permette di isolare un modulo software simulando l'interazione con altre componenti. Risulta necessario rendere l'ambiente **stabile**, ossia:
- fornire valori consistenti per rendere i test ripetibili;
- fornire un ambiente controllato per testare comportamenti attesi e inattesi.

Per l'environment si costruiscono dei fantocci per un modulo. Si parla generalmente di **driver** e di **stub**. Un driver è un oggetto che sostituisce un altro oggetto nel sistema; ha il ruolo di simulare l'ambiente per il modulo da testare. Lo stub invece è un altro modulo che veniva utilizzata dal modulo oggetto di test. In generale:
- moduli che dipendono dall'oggetto di test sono i driver;
- moduli da cui dipende l'oggetto di test sono gli stub.

### Integrazione del sistema e testing
Per il testing del sistema e dell'interazione tra i moduli si possono seguire due approcci: **top-down** e **bottom-up**.\
Generalmente se abbiamo un sistema con diversi moduli, si comincia a sviluppare i moduli che non presentano dipendenze: questi moduli possono essere sviluppati in maniera del tutto indipendente e non richiedono dipendenze.\
È anche possibile testare un sistema dall'alto verso il basso, possibile quando non si hanno strutture di dipendenze ad albero. Solitamente si preferisce uno sviluppo e un testing bottom-up.

### Automazione dei Test, JUnit
Strumenti quali JUnit e TestNG vengono utilizzati per l'automazione del testing. Altri software, come Randoop, EvoSuite e Ttuples possono essere utilizzati per la generazione di test. Utilizzeremo adesso JUnit.

Per scrivere classi per i test, ogni metodo viene annotato con la flag `@Test`. Ogni metodo di test viene trovato ed eseguito automaticamente dal framework utilizzando la riflessione computazionale. Per confrontare l'output con quello atteso, si utilizzano le **asserzioni**: la valutazione viene automata e il riscontro viene analizzato da JUnit.

La classe `Assert` fornisce i metodi usati per la valutazione dei test:
- `assertTrue(boolean condition)` verifica se `condition` è `true`;
- `assertEquals(expected, calculated)` verifica se `calculated` è uguale a `expected`. In caso contrario viene generata un'eccezione. `assertEquals` ammette un terzo parametro opzionale `delta` per una tolleranza di valore (ossia verifca se tra `expected` e `calculated` vi è una differenza minore o uguale a `delta`);
- `fail(String message)` viene utilizzato per testare le situazioni di eccezione e di failure del programma.

Poiché i metodi di test possono essere molti, è meglio avere un nome significativo per metodo di test. Per ogni test è meglio scrivere un metodo a parte.

Quando eseguiamo i test, per ogni metodo di test eseguito la classe viene istanziata nuovamente, in quanto i metodi vengono eseguiti in isolamento in un ambiente consistente. Ogni test lavora su istanze differenti della classe in cui è definito.

Esistono alcune annotazioni che possiamo utilizzare per creare delle **test suite**:
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({ConsumerTest.class, SumTest.class})
public class TestSuiteA{}

@RunWith(Suite.class)
@Suite.SuiteClasses({ConsumerTest.class, SumTest.class})
public class TestSuiteB{}

@RunWith(Suite.class)
@Suite.SuiteClasses({TestSuiteA.class, TestSuiteB.class})
public class MasterTestSuite{}
```
Notiamo nell'esempio anche che è possibile raggruppare test suite per formare una test suite di livello superiore. 

Altre annotazioni importanti per i metodi sono:
- `@Before`, che annota un metodo in modo tale che esso venga chiamato prima di ogni test. È utile quando bisogna inizializzare lo stato di istanza della classe di test, così che tutti i test partano dalle stesse condizioni;
- `@After`, che annota metodi chiamati dopo l'esecuzione di ogni test. Viene usato per operazioni di chiusura o finali;
- `@BeforeClass` annnota un metodo statico da eseguire prima del lancio di test di una classe. A differenza di `@Before`, il metodo viene eseguito **una volta sola per classe di test**.
- `@AfterClass` annota un metodo statico da eseguire dopo il lancio di test di una classe con le modalità di esecuzione di `@BeforeClass`.

Utilizzare le flag senza `Class` permette di resettare l'environment dopo o di impostarlo prima di ogni metodo di test. Con `Class`, i metodi sono eseguiti una tantum.

### Mock Objects
Poiché i test di unità devono testare la correttezza di una singola classe, si utilizzano degli **oggetti fantoccio**, ossia degli oggetti finti che restituiscono valori specifici in virtù del test. Questi oggetti implementano la stessa interfaccia del vero collaboratore, ma il comportamento dei metodi restituisce un risultato predefinito come uno stub.

In Java la libreria Mockito semplifica la scrittura di test con le annotazioni `@Mock`. 

