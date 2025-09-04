# Espressioni lambda
Una funzione lambda è un blocco di codice che descrive una funzione anonima non associata ad alcuna classe. Solitamente gli algoritmi vengono incapsulati in un metodo in cui ogni parametro è tipizzato e identificato da un nome. La sintassi di una funzione lambda è la seguente:
```java
(lista_parametri) -> {corpo_lambda}
```
Tipicamente le funzioni lambda sono semplici, leggibili e facili da debuggare. Il costrutto rappresenta una funzionalità che è un **oggetto di prima classe**. La funzione si dice anonima perché non si sa qual è il nome, ma si può comunque passare o assegnare a qualunque variabile.\
Le funzioni lambda inoltre supportano parametri astratti:
```java
(x, y) -> x+y;
```
La funzione lambda soprastante definisce la somma di due elementi `x, y` il cui tipo è sconosciuto. Funzioni del tipo:
```java
(x, y) -> x.length() + y.length();
```
Richiederebbe che `x` e `y` siano stringhe per funzionare correttamente (o qualunque ADT che supporti `length()`). Se i parametri passati non hanno il metodo `length()`, tramite una **deduzione di tipo a tempo di compilazione** viene segnalato un errore e il bytecode non viene prodotto.\
Poiché la funzione lambda è un oggetto di prima classe, il qualificatore `this` fa riferimento al contesto in cui è definita la funzione lambda. In generale una funzione lambda può utilizzare i parametri dello scope in cui è definita:
```java
class Test{
    private int testInt;
    void foo(ArrayList<Integer> arr, int p) {
        arr.forEach(d -> System.out.println(d + p));
    }
}
```
## Interfacce funzionali
Un'**interfaccia funzionale** è un'interfaccia che ha **un solo metodo astratto** (ma non obbligatoriamente un solo metodo). Si utilizza l'annotazione `@FunctionalInterface` per specificare al compilatore di controllare se effettivamente si ha un solo metodo astratto nell'interfaccia definita; se ciò non accade, viene generato un errore a tempo di compilazione.

Abbiamo già visto le interfacce funzionali `Consumer` e `Comparator`. Introduciamo `Predicate<T>` come interfaccia funzionale che implementa il metodo `test(T)`. Il metodo astratto restituisce un booleano e riceve un elemento di tipo `T`. 

Utilizzare le interfacce funzionali permette di definire on-spot delle classi che implementano la funzione `test(T)` e passarle a funzioni:
```java
public void elaborate(Test t, Predicate<Test> func) {
    if(func.test(t)) {
        //...
    }
}

elaborate(t, new Predicate<Test>() {
    @Override 
    public boolean test(Test t) { t.getValue() == 3; }
});
```
Possiamo anche fare di meglio utilizzando le espressioni lambda:
```java
public void elaborate(Test t, Predicate<Test> func) {
    if(func.test(t)) {
        //...
    }
}

elaborate(t, t -> t.getValue() == 3);
```
Interfacce funzionali note sono:
- `Predicate<T>`, che produce un risultato booleano sulla proprietà di un oggetto. Contiene `boolean test()`;
- `Consumer<T>`, che consuma un oggetto ricevuto eseguendo un azione. Contiene `void accept(T)`;
- `Function<T, R>`, che trasforma `T` a `R`, creando una corrispondenza $T\rightarrow R$. Contiene `R apply(T)`;
- `Supplier<T>`, che fornisce un'istanza di `T`. Un `Supplier` non è altro che un factory. Contiene `T get()`;
- `UnaryOperator<T>`, funzione da `T` a `T`. Contiene `T apply(T)`;
- `BinaryOperator<T>`, funzione che associa `(T, T)` a `T`. Contiene `T apply(T, T)`;
- `Factory<T>`, che fornisce un metodo per istanziare oggetti di tipo `T`. Contiene `T create()`.

Queste interfacce sono presenti in `java.util.function`.

Un'interfaccia funzionale può anche prevedere l'implementazione di metodi ereditati dalla classe `Object`:
```java
public interface TestInterface {
    String printInfo(TestObject test);
    boolean equals(Object obj);
}
```
Ogni classe che implementa `TestInterface` dovrà implementare `equals()`. Questa è ancora un'interfaccia funzionale in quanto `equals(Object obj)` è ereditato da `Object`, pertanto è solo un override. 

Anche se ad una funzione viene passata un'espressione lambda, il compilatore analizza il tipo e l'assegnazione. Ad esempio:
```java
void consumeStudent(Consumer<Student> s) {
    // logic of consuming...
}

consumeStudent(s -> System.out.println(s.getName()));
consumeStudent( () -> System.out.prinln("Test"));
```
Mentre il primo passaggio a funzione è legale, il secondo non viene accettato dal compilatore: l'espressione lambda rappresenta una funzione che non prende in input nessun parametro, ma l'interfaccia funzionale `Consumer<Student>` prevede un metodo `accept(Student)`, che richiede quindi un parametro.

L'aspetto importante nell'utilizzo delle interfacce funzionali è ricordare la firma della funzione dichiarata all'interno dell'interfaccia. In questo modo non abbiamo realmente bisogno di istanziare classi, ma possiamo utilizzare classi anonime o ancora meglio utilizzare delle espressioni lambda, rispettando tuttavia il numero di parametri in ingresso e il tipo in uscita (per le espressioni il tipo può anche essere dedotto).

## Riferimenti a metodi e costruttori
In java è possibile passare un metodo esistente come parametro basta passare il nome del metodo. Per passare i metodi è necessario utilizzare l'operatore di **dereferenziazione** `::`.\
Consideriamo il seguente esempio:
```java
class Person {
    private String name;
    private int age;

    public int getAge() { return this.age; }
    public String getName() { return this.name; }

Person[] people = ...;
Comparator<Person> byName = Comparator.comparing(Person::getName);
Arrays.sort(people, byName);
}
```
L'interfaccia `Comparator` presenta un metodo statico `comparing` con la seguente firma:
```java
static <T,U extends Comparable<? super U>> Comparator<T> comparing(Function<? super T,? extends U> keyExtractor)
```
Tale metodo statico accetta una funzione che a partire da un oggetto di tipo `T` restituisce una chiave di ordinamento di tipo `U` e restituisce un `Comparator<T>` che esegue confronti in base alla chiave. Il `Comparator` viene utilizzato per ordinare gli array tramite la funzione `Arrays.sort`:
```java
public static <T> void sort​(T[] a, Comparator<? super T> c)
```
Invece di istanziare un oggetto `Function` possiamo passare un **riferiemnto ad un metodo**. Il riferimento in questo caso viene detto **riferimento ad un metodo di istanza di un oggetto arbitrario**. Esistono quattro tipi di riferimenti di metodo:
- **riferimento ad un metodo statico**: si utilizza il nome di una classe e la dereferenziazione di un metodo statico della classe. Da notare che questo vale anche quando un metodo non statico di una classe non utilizza alcuna variabile di istanza;
- **riferimento ad un metodo di istanza di un oggetto specifico**: si utilizza il nome dell'oggetto e la dereferenziazione di un metodo;
- **riferimento ad un metodo di istanza di un oggetto arbitrario di un tipo specifico**: si utiliza il nome del tipo e la dereferenziazione di un metodo;
- **riferimento ad un costruttore**: si utilizza il nome della classe e la dereferenziazione di `new`.

## Metodi default
Per introdurre nuove funzionalità senza dover cambiare il codice pre-esistente, Java 8 introdusse i **metodi default**. Le librerie pre-esistenti possono ereditare le funzionalità: il metodo default infatti viene ereditato dalle altre interfacce come già implementato. Utilizzare i metodi default quindi permette di inserire nuove funzionalità nelle vecchie interfacce senza cambiare il codice, rendendo quindi il codice retro-compatibile. Ad esempio, l'interfaccia `Iterable` implementa `forEach(Consumer c)` come metodo default.

La gerarchia dei tipi in Java deve consentire un solo padre, evitando quindi l'ereditarietà multipla. Tuttavia, una classe potrebbe implementare più interfacce. A livello di ereditarietà di tipi, questo non crea ambiguità: una classe sta solo includendo più set di funzionalità. Ma dal punto di vista implementativo si possono avere conflitti: se nelle due interfacce implementate esistono metodi default con lo stesso nome, si ereditano più implementazioni dello stesso metodo. Invocare la funzionalità col nome, il nome è uno, ma le implementazioni sono molteplici. Per risolvere il conflitto, è necessario utilizzare il nome dell'interfaccia e invocare con `super`:
```java
interface I1 {
    public default void display() {
        System.out.println("Interfaccia 1");
    }
}

interface I2 {
    public default void display() {
        System.out.println("Interfaccia 2");
    }
}

public class Test implements I1, I2 {
    public void display() {
        I1.super.display();
        I2.super.display();
    }
}
```
