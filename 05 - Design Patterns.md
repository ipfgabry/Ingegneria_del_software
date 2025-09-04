# Principi di design 
Nella fase di progettazione del nostro software, ogni entità software identifica un'entità concreta. Gli ADT che definiamo devono avere un senso in virtù della realtà che modelliamo.
## Principio della singola responsabilità
L'identificazione di un'entità dev'essere tale da delineare un insieme ben focalizzato di oggetti. Il **principio di singola responsabilità** afferma quindi che:
> *Ogni metodo deve svolgere una singola attività in relazione al suo ADT, e ogni classe deve rappresentare un singolo concetto.*
## Principio di programmazione riferita a interfacce
> *Bisogna programmare riferendosi ad **interfacce**, non ad **implementazioni**.*

Ad esempio, supponendo nel nostro software di utilizzare un algoritmo implementato in una classe, che a sua volta implementa funzionalità tramite interfaccia o classe astratta, conviene far riferimento all'interfaccia o classe astratta. In questo modo se in un secondo momento si introduce un secondo modulo software che utilizza la stessa interfaccia, non vi è la necessità di riscrivere il modulo software del client. 
A livello pratico, gli oggetti definiti nel modulo client i tipi associati agli identificatori sono i tipi delle **interfacce** o delle **classi astratte**.
### Librerie di Java
Il principio esposto precedentemente è stato ampiamente utilizzato per i package java. Un esempio sono le **collection** di Java. La libreria Java introduce l'interfaccia `Collection`, un'interfaccia generica per **aggregati generici**. Ad esempio:
```java
	Collection mySet = new <type>;
```
identifica `mySet` come un oggetto che può essere di qualsiasi tipo di collection. Chiaramente `<type>` dev'essere una **implementazione** dell'interfaccia, come un `HashMap`, `List`, `Vector` e così via. 
L'interfaccia `Collection` viene **estesa** con le definizioni delle interfacce `List`, `Set` e `Map`. Si crea così un albero le cui foglie sono le implementazioni delle interfacce:
![[Pasted image 20250319083621.png]]
## Favorire la composizione all'ereditarietà
Importante è anche il seguente principio:
> *Bisogna sempre favorire la composizione all'ereditarietà.* 

Abbiamo già visto che l'ereditarietà consiste nella definizione di un tipo `B` derivato da un tipo `A` con un legame di tipo, mentre la composizione prevede nella definizione del tipo `B` un oggetto di tipo `A`. Questo legame **non** è un legame di tipo, permettendo l'utilizzo delle funzionalità del tipo `A` senza che vi sia un legame concettuale. Questo legame è più dinamico, permettendo un approccio più flessibile: si può decidere quale oggetto è usato da un altro oggetto in maniera dinamica durante l'esecuzione, invece di deciderlo in fase di design. Il comportamento quindi cambia durante l'esecuzione.
Tale approccio favorisce anche la **coesione** e l'abbassamento degli **accoppiamenti**.
### Coesione
Nel momento della progettazione di una classe, possiamo facilmente individuare un metodo candidato ad essere **fuori posto** in virtù degli attributi che caratterizzano gli oggetti della classe da definire. I metodi devono lavorare sugli attributi della classe. Più i metodi lavorano sugli stessi attributi della classe, più sono legati tra loro.
Ad esempio, una classe `Calcolatrice` potrebbe permettere di memorizzare un valore salvato nella calcolatrice e calcolarne il quadrato. Il metodo `salva()` e il metodo `calcolaQuadrato()` sono legati tra loro. 
La **coesione** esprime quanto i metodi della classe siano dipendenti tra loro (**coesi**). Le dipendenze derivano dall'usare gli stessi attributi. 
### Accoppiamento
Se la coesione è una misura interna relativa ai metodi della classe, l'accoppiamento è una misura definita in virtù delle relazioni tra un modulo software e altri moduli software (ad esempio, come si divide il sistema in sottosistemi). Vogliamo che il nostro software presenta **meno dipendenze possibili**. 
Si può parlare di accoppiamento anche nello stesso modulo software, ed in tal caso si individuano le dipendenze tra le classi che compongono il nostro software. La composizione, in particolare, favorisce un accoppiamento **dinamico**. 
## Principio dei tipi parametrizzati
I tipi generici permettono la composizione dinamica all'interno del nostro software. Permette di ottenere maggiore flessibilità, creando composizioni a tempo di esecuzione. 
# Design Pattern
Un **design pattern** fa riferimento all'organizzazione del software che viene scelta per risolvere problemi che nascono dal modo in cui dobbiamo far interagire due classi tra di loro. Ogni pattern descrive un problema che si presenta nel nostro ambiente e che descrive la soluzione del problema in modo tale da riutilizzare la soluzione proposta per problemi dello stesso tipo.
Ogni design pattern è costituito da:
1. nome del pattern;
2. problema che descrive quando applicare il pattern;
3. la soluzione utilizzata che descrive gli elementi che costituiscono il design, le relazioni, le responsabilità e le collaborazioni tra gli elementi del design;
4. le conseguenze, che descrivono i trade-off nell'utilizzo del pattern.

Ogni pattern risolve un singolo problema. Nello stesso contesto è anche possibile usare più pattern insieme, aumentando i benefici. 
## Catalogo dei pattern Gang of Four (GoF)
Il catalogo **GoF** è un catalogo che elenca in base all'ambito e allo scopo di un problema le soluzioni proposte. Più precisamente, è un insieme di 23 design patterns introdotti nel libro *Design Patterns: Elements of Reusable Object-Oriented Software*, scritto dagli autori Erich Gamma, Richard Helm, Ralph Johnson e Joh Vlissides (da cui appunto **Gang of Four**, che indica i quattro autori). I pattern sono classificati in tre categorie: 
- **creational patterns**, pattern orientati al processo di creazione di oggetti fornendo maggiore flessibilità per il programma;
- **structural patterns**, riguardanti la composizione tra classi. Utilizzano l'ereditarietà per comporre interfacce e descrivono i modi per comporre gli oggetti e ottenere nuove funzionalità;
- **behavioral patterns**, riguardanti la comunicazione tra oggetti.

![[Pasted image 20250319131614.png]]
I pattern sono anche in stretta relazione tra loro, come mostra il grafo sottostante:![[Pasted image 20250319131744.png]]
# Pattern Iteratore
Il problema ricorrente è la navigazione di una struttura dati possibilmente eterogenea. Il problema non è immediatamente percepibile: se una struttura dati è omogenea, è molto semplice scorrere gli elementi. 
Per poter navigare un aggregato (possibilmente eterogeneo), dobbiamo essere in grado di astrarre il tipo di elemento che stiamo navigando. Dobbiamo allora immaginare la navigazione come una **funzionalità astratta** e **separata** dall'aggregato. 
Abbiamo allora un aggregato e un iteratore astratti (`Aggregate` e `Iterator`), e le classi `ConcreteAggregate` e `ConcreteIterator` che sono implementazioni delle definizioni astratte. All'interno dell'astrazione `Aggregate` abbiamo un metodo astratto `createIterator()`, mentre in `Iterator` avremo metodi quali `first()`, `next()`, `isDone()`, `currentItem()`. Ad esempio, `ConcreteAggregate` potrebbe essere uno stack, mentre il `ConcreteIterator` uno `StackIterator`. La navigazione dello stack diventa un'**entità separata dallo stack**. A livello implementativo lo `StackIterator` sarà realizzato con una **composizione** col tipo `Stack`. Notiamo tuttavia che queste implementazioni sono dipendenti tra loro, **ma non le loro astrazioni**: il nostro client farà riferimento all'astrazione, permettendo **riusabilità**. Un esempio pratico è il ciclo enhanced `for`, che utilizza implicitamente un iteratore.
Il vantaggio nell'utilizzo dell'iteratore inoltre è quello di permettere **attraversamenti multipli** di aggregati di oggetti. Ogni attraversamento infatti è un'istanza di `ConcreteIterator`, pertanto possiamo avere attraversamenti multipli utilizzando diversi iteratori. Inoltre, permette di avere un'interfaccia uniforme anche per aggregati che sono diversi tra loro (stack, liste, ecc.).
Possiamo anche avere un'astrazione di un caso particolare, ossia il caso in cui abbiamo un **aggregato vuoto**. Tale astrazione è l'**iteratore nullo**, che semplicemente nel metodo `isDone()` restituisce automaticamente `true`.
## Esempio pratico: Framework Collections
Il framework collections è un framework di Java che comprende un insieme di **iterabili** e i relativi iteratori. Si tratta di un'architettura non definita per rappresentare e manipolare collezioni in maniera indipendente dall'implementazione. 
All'interno del framework si trova anche la definizione di `ArrayList` e `Vector`. Entrambe sono implementazioni dell'interfaccia `List`, ma differiscono per alcuni aspetti:
1. `ArrayList` non è sincronizzata, mentre `Vector` sì. Sincronizzato significa che in un ambiente multithread, gli altri thread aspettano che il thread attuale rilasci il lock sull'oggetto. Di conseguenza, `ArrayList` è più veloce di `Vector`;
2. `ArrayList` incrementa del 50% la dimensione dell'array quando vi è un overflow, mentre `Vector` la incrementa del 100%;
3. `ArrayList` utilizza l'iteratore per attraversare la struttura, ma il vettore può anche utilizzare `Enumeration` come interfaccia. 

L'iteratore di `ArrayList` ci permette di scorrere gli elementi presenti all'interno della struttura astraendo la rappresentazione della collezione. Potremmo infatti avere i seguenti codici:
```java
// senza iterator
void print(ArrayList list) {
	for(int k = 0; k < list.size(); k++) {
		System.out.println(list.get(k));
	}
}

void print(LinkedList list) {
	Node current = list.first();
	while(current.hasNext()) {
		System.out.println(current);
		current = current.next();
	}
}

// con iterator
void print(Collection list) {
	Iterator items = list.iterator();
	while(items.hasNext()) {
		System.out.println(items.next());	
	}
}
```
Chiaramente l'iteratore ci permette, **a prescindere dal tipo di collezione implementato**, di iterare sulla collezione.
## Schema del pattern
Intento: Attraversare gli elementi di un aggregato senza esporre la sua struttura, nascondendo al client i dettagli della classe visitata. 
Idea: togliere le funzionalità di accesso e attraversamento dall'aggregato e inserirle in un nuovo oggetto **`Iterator`**. 
Risulta possibile eseguire molteplici scorrimenti in parallelo utilizzando più iteratori.
Lo schema del pattern è il seguente:
![[Pasted image 20250319105800.png]]
### Astrazione stream
Ogni oggetto iterabile si può astrarre come **`Stream`**, un'interfaccia ispirata alla programmazione funzionale che incorpora al suo interno il concetto di aggregato astratto. L'aggregato può essere visto come stream per la sua navigazione.
L'interfaccia `Stream`, insieme all'interfaccia `Iterable`, presentano un metodo `forEach()` per iterare su tutti gli elementi presenti nella collection. Supponendo di avere:
```java
	List listOfStrings = new Vector<String>();
```
Possiamo iterare sugli elementi con **iteratore implicito** (enhanced `for`):
```java
	for(String s : listOfStrings) System.out.println(s);
```
oppure possiamo usare il metodo `forEach()` dell'interfaccia `Iterable`:
```java
	listOfStrings.forEach(System.out::println);
```
o ancora, `forEach()` dell'interfaccia `Stream`:
```java
	listOfStrings.stream().forEach((c) -> System.out.println(c));
```
Con gli stream abbiamo la possibilità di scrivere cicli sotto forma di singola espressione, non per rappresentare operazioni sequenziali, ma per rappresentare operazioni su aggregati. Possiamo così eseguire **operazioni parallele** sull'aggregato.