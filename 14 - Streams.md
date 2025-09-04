# Streams
L'interfaccia Stream è un'interfaccia che raccoglie operazioni astratte che operano su un flusso o aggregato di oggetti. Gli stream forniscono dei blocchi funzionali per processare meglio gli aggregati:
- utilizzando un'iterazione interna. Il ciclo scompare del tutto, in quanto non è necessariamente un ciclo, ma un'applicazione a tutto l'aggregato di un'operazione;
- supportando il paradigma di **programmazione funzionale**. Passiamo dalla definizione imperativa di algoritmi, che prevede la precisazione dell'ordine di esecuzione, ad una definizione funzionale, in cui l'ordine di esecuzione non è definito così come accade quando si valuta un'espressione matematica.

## Elementi di uno stream
Gli stream sono costituiti da tre tipi di elementi:
- una **source**;
- una o più **operazioni intermedie**;
- un'**operazione terminale**.

L'operazione terminale potrebbe dare vita ad un aggregato di oggetti (non necessariamente uguale all'aggregato di partenza) oppure può essere una riduzione ad un elemento più semplice. Le operazioni intermedie, tuttavia, non lavorano su un aggregato di oggetti, bensì un oggetto di tipo `Stream`. La sorgente quindi permette di generare uno stream. Ad esempio, una `Collection` può essere trasformata in uno `Stream` dopo una trasformazione. Tale processo di trasformazione è l'invocazione del metodo `stream()` sulla collezione. Lo stream esiste solo durante l'esecuzione delle operazioni intermedie.
### Dettagli interni
All'interno dell'interfaccia `Collection` è presente un metodo `stream()` che trasforma l'oggetto in uno `Stream` che ha accesso all'oggetto `Collection`. Lo `Stream` creato incapsula al suo interno la `Collection`, pertanto le funzionalità applicate allo stream si riflettono sull'aggregato incapsulato.\
Molti metodi all'interno di `Stream`, inoltre, restituiscono uno `Stream`, permettendo una **concatenazione delle operazioni**:
```java
result = albums.stream().filter(track -> track.getLength() > 300).map(Track::getName).collect(Collectors.toSet());
```
La funzione `filter()` prende in input un `Predicate`, filtrando gli elementi dello stream in base al predicato passato. Dopo aver applicato un filtro, utilizziamo la funzione `map()`, che prende in input una `Function`, per mappare, ossia associare, ad ogni traccia il suo nome. Infine, il metodo `collect` esegue un assemblaggio dello stream in una collezione, utilizzando la funzionalità `Collectors.toSet()`. Si ottiene quindi un insieme di tracce che hanno una lunghezza maggiore di 300.\
Dietro le quinte, tuttavia, non è detto che accada quanto illustrato. In realtà, infatti, **nessuna delle operazioni intermedie viene eseguita nell'ordine specificato dalla riga di codice**. L'esecuzione in realtà comincia dalla operazione terminale: le operazioni intermedie fungono da specifica per il flusso di oggetti. 

## Creare uno stream
Abbiamo visto che in `Collection` è presente un metodo `stream()`. Esiste anche un metodo `parallelStream()`, che fornisce uno stream gestito in maniera parallela.\
In `Arrays` esiste `stream()` poiché anche un array è un aggregato di dati. Il metodo fornisce uno stream sequenziale.

Per creare stream a partire da valori, si utilizza `of()`:
```java
Stream<Integer> digits = Stream.of(3,1,4,5,6,2);
```
Uno stream vuoto può essere creato con `empty()`:
```java
Stream<Student> ss = Stream.empty();
```
Si possono generare stream a partire da funzioni:
```java
Random random = new Random();
Stream<Integer> randomNumbers = Stream.generate(random::nextInt);
```
`random` permette la generazione di numeri pseudo-casuali. Per questo motivo utilizziamo il riferimento di un metodo di istanza: utilizzare `nextInt` in maniera statica implica l'utilizzo di uno stesso seed di generazione che ad ogni istanza genera la stessa sequenza di numeri. Il seed è una variabile di istanza, pertanto istanziamo un oggetto `random` di tipo `Random`.\
In alternativa a partire dagli array è possibile utilizzare il metodo `of()` per creare uno stream:
```java
Integer[] arr = {1,2,3,4,5};
Stream<Integer> digitStream = Stream.of(arr);
```
Abbiamo già visto che con `Collection` esiste il metodo `stream()`:
```java
Set<Student> ss = new LinkedHashSet<>();
Stream<Student> stream = ss.stream();
```
Si possono anche creare stream a partire dai file:
```java
Stream<String> stream = Files.lines(path);
```
Infine, si possono concatenare gli stream:
```java
Stream newStream = Stream.concat(stream, randomNumbers);
```

## Builder pattern
Per creare uno stream, all'interno dell'interfaccia è presente un metodo `builder()` che restituisce un **builder** per uno `Stream`. Il metodo si basa sul **builder pattern**, che risolve il problema dell'assemblaggio di una struttura composta anche da altre strutture composte secondo una logica corretta. In questo caso possiamo utilizzare il metodo `builder()` per costruire lo `Stream`. Il metodo `add()` permette di aggiungere elementi allo `Stream`. Invocare infine il metodo `build()` permette di restituire lo `Stream` di oggetti. Da notare che prima di invocare `build()` è necessario passare tutti gli oggetti con `add()`. Inoltre, se passiamo un aggregato con `add()`, l'aggregato è un elemento dello stream, permettendo la creazione di **stream compositi**. Quando si processa uno stream di elementi, non è detto che il singolo elemento dello stream sia semplice. 

## Stream primitivi e infiniti
Può darsi che invece di avere un flusso di oggetti si abbia un flusso di dati elementari. Questo flusso di dati è comunque trattato dall'operazione terminale correttamente, ma bisogna stare attenti a distinguere `int` e `Integer`: il primo è un tipo elementare, il secondo è una classe. L'interfaccia generale è quindi `Stream`, ma ci sono delle istanze specifiche di `Stream`, come `IntStream`, `DoubleStream`, e così via.

Ad esempio, le funzioni `range(int, int)` e `rangeClosed(int, int)` permettono di generare uno stream dal valore di inizio al valore di fine (`rangeClosed()` include il valore finale).

Inoltre è possibile anche generare uno **stream infinito** a partire da un `Supplier`:
```java
generate(IntSupplier)
```
Uno stream infinito può essere utile per generare una sequenza numerica. Uno stream infinito può anche essere creato con `iterate`:
```java
Stream<Integer> infiniteStream = Stream.iterate(0, i -> i + 1)
```
Lo snippet soprastante genera una sequenza strettamente crescente di numeri. Chiaramente se vogliamo utilizzare il metodo `collect()` per assemblare lo stream è necessario limitare il numero di valori, altrimenti **si ottiene un'esecuzione infinita del programma**:
```java
infiniteStream.limit(10).collect(Collectors.toList());
```
Lo snippet prende i primi 10 valori.

## Iterazione con `Stream.iterate`
La funzione `iterate()` dell'interfaccia `Stream` permette di ottenere uno stream ordinato:
```java
static <T> Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next)
```
La funzione applica iterativamente la funzione `next` passata come funzione unaria. L'iterazione continua fino a quando `hasNext` non restituisce `false`. Produce la stessa identica sequenza del ciclo for:
```java
for(T index = seed; hasNext.test(index); next.apply(index)) {
    ...
}
```
## Stream sequenziali e paralleli
Una collection genera tramite il metodo `stream()` uno stream sequenziale. Uno stream **parallelo** può essere utilizzato per eseguire operazioni in parallelo. Le operazioni chiaramente vengono eseguite o esclusivamente in parallelo o esclusivamente in sequenza:
```java
List<Person> persons = ...;
List<Student> students = persons.stream().parallel().filter(p->p.getAge() > 18).sequential().map(Student::new).collect(Collectors.toCollection(ArrayList::new));
```
Uno stream sequenziale, specificato con `sequential()`, viene eseguito utilizzando un unico thread. Uno stream parallelo invece viene gestito utilizzando più thread, ma causa più overhead a causa della gestione di questi e dei costi del merging dei risultati.
## Operazioni intermedie
Le operazioni intermedie solitamente prendono in input delle espressioni lambda, o in generale delle funzioni che sono **stateless**. L'iterazione è nascosta all'interno dell'implementazione dello stream nella libreria `Stream`.

Alcune operazioni intermedie note sono:
- `filter(Predicate)`: fitlra gli elementi in virtù del risultato di `Predicate`. Se il predicato contiene delle congiunzioni logiche, conviene utilizzare `filter()` per ogni singolo predicato, in quanto si ha un confronto semplificato, poiché gli elementi eliminati dal filtro per il primo predicato non verranno considerati per il filtro per il secondo predicato. Inoltre poiché l'ordine dei termini non è specificato per le operazioni intermedie, può darsi che il confronto separato filtri gli elementi più rapidamente, senza cambiare il risultato;
- `map(Function)`: trasforma un elemento usando `Function`;
- `mapToInt, mapToDouble, mapToLong`: utilizzate come `map()`, ma generano uno stream di tipi primitivi;
- `flatMap(Function)`: trasforma ogni elemento in un **sottostream** di zero o più elementi. Semplicemente, mentre `map()` associa ad un elemento un unico elemento, `flatMap()` associa ad un elemento anche più elementi;
- `peek(Consumer)`: esegue un azione su ogni elemento, utile per il debugging;
- `distinct()`: escluse elementi duplicati;
- `sorted(Comparator)`: restituisce uno stream ordinato. Se il `Comparator` non è specificato, restituisce un ordine naturale degli elementi;
- `limit(long)`: limita il numero di elementi dallo stream;
- `skip(long)`: salta gli elementi iniziali, il cui numero è specificato dal parametro.

## Operazioni terminali
Le operazioni terminali terminano la pipeline delle operazioni eseguite sullo stream. Il processamento viene eseguito quando viene chiamata un'operazione terminale, ottimizzando la pipeline. Questa tecnica prende il nome di **lazy evaluation**. Si rimuovono inoltre operazioni ridondanti. Infine, le operazioni sono eseguite in parallelo.

`collect(Collectors)` accumula lo `Stream` in un aggregato. La collezione può essere specificata utilizzando metodi quali `Collectors.toList()` oppure `Collectors.toCollection(constructor)`, dove `constructor` è il riferimento al costruttore di una `Collection`. Esistono anche computazioni per la somma, come `Collectors.summingInt(int_value)`, oppure le stringhe `Collectors.joining(str)`, dove `str` indica di concatenare le stringhe utilizzando la stringa `str`.

È anche possibile partizionare elementi in virtù di un `Predicate`:
```java
Map<Boolean, List<Student>> passingFailing = students.stream().collect(Collectors.partitioningBy(s -> s.getGrade() >= 70))
```
La partizione viene quindi utilizzata per associare ad ogni studente un valore booleano in virtù della condizione, che in questo caso è avere un voto che sia maggiore o uguale a 70.

### Optional
Per eliminare la `NullPointerException` si utilizza la classe `Optional<T>`. Operazioni come `min()` e `max()` potrebbero infatti non restituire un risultato diretto: se lo stream è vuoto, non si può determinare qual è l'elemento minimo o massimo.\
Un `Optional<T>` è un container di elementi di tipo `T` che ha 0 o 1 elemento, ma non è mai `null`. 

`Optional` include i metodi:
- `empty()` per creare un `Optional` vuoto;
- `of(T value)` restituisce un `Optional` che contiene `value`. Se `value` è `null`, il metodo causa una `NullPointerException`;
- `ofNullable(T value)` ha lo stesso scopo di `of()` ma se `value` è uguale a `null` restituisce un `Optional` vuoto;
- `get()` restituisce il valore contenuto in `Optional`. Se `value` è vuoto causa una `NoSuchElementException`;
- `isPresent()` restitusce se è presente un valore;
- `orElse(T defaultValue)` restituisce un valore default se `value` è empty;
- `orElseGet(Supplier<? extends T> defaultSupplier)` ha lo stesso significato di `orElse` ma è un `Supplier` che dà il valore default;
- `orElseThrow(ExceptionSupplier)` causa un'eccezione dal supplier se l'optional è vuoto;
- `ifPresent(Consumer<T> consumer)` esegue le funzionalità fornite dal `consumer` con il value se `value` non è vuoto.

### `reduce()`
Oltre a `collect` esiste anche `reduce(BinaryOperator accumulator)` per eseguire una **riduzione**, ossia per raccogliere i dati dello stream in un unico risultato. `accumulator` prende in input un risultato parziale, il successivo elemento e restituisce un risultato parziale sotto forma di `Optional<T>`. Quindi `accumulator` è una funzione del tipo `(T, T) -> T`. Ci sono anche altre due versioni:
- una versione prende in input un valore iniziale;
- un'altra versione prende in input anche una funzione `(T, U) -> U` per generare un risultato con tipo `U` invece che `T`.

### Altre operazioni terminali
Altre operazioni terminali utilizzate sono:
- `count()` per contare gli elementi;
- `average()`, `sum()` per eseguire media e somma;
- `allMatch()`, `anyMatch()` e `noneMatch()` per restituire risultati booleani sulla base di un `Predicate` dato in input. `allMatch()` restituisce `true` se tutti gli elementi soddisfano il predicato, `anyMatch()` se ne esiste almeno uno e `noneMatch()` se nessuno soddisfa il predicato;
- `forEach()` esegue l'operazione fornita dal consumer passato al metodo, restituendo `void`;
- `forEachOrdered()` esegue la stessa cosa di `forEach()` ma seguendo un ordine.
  
