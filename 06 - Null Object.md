Supponiamo di voler implementare un albero binario di ricerca. Allora potremmo scrivere:
``` java
public class BinaryNode {
	int key;
	Node left = null;
	Node right = null;

	public boolean includes(int value) {
		if (value == key) return true;
		else if (value < key && left == null) return false;
		else if (value < key) return left.includes(value);
		else if (value > key && right == null) return false;
		else return right.includes(value);
	}
}
```
Il problema di questo codice, che risulta più difficilmente leggibile, risiede nel fatto che **controlliamo più volte il caso del valore nullo**. Tale problema può essere risolto utilizzando il pattern **Null Object**.
Il **Null Object pattern** consiste nell'implementazione di un **Null Object**. Ad esempio:
```java
if (customer == null) plan = new BasicBillingPlan();
else plan = customer.getPlan();
```
Diventa:
```java
plan = customer.getPlan();
```
dove in una classe `NullCustomer` si ha che la funzione `getPlan()` viene restituito `new BasicBillingPlan()`:
```java
Class NullCustomer {
	getPlan() { return new BasicBillingPlan(); }
}
```
In questo modo la funzione `getPlan()` può essere invocata anche se `customer` è un oggetto nullo. 
In Java gli identificatori senza assegnazione ricevono automaticamente valore nullo. Invocazioni di metodi a partire da identificatori nulli causano eccezioni. Di conseguenza, invece di lasciare che Java assegni `null` a identificatori in maniera automatica, assegniamo un'istanza di un `NullObject` che definiamo.
In questo modo il nostro codice diventa:
``` java
public class BinaryNode extends Node{
	int key;
	Node left = new NullNode();
	Node right = new NullNode();

	public boolean includes(int value) {
		if (value == key) return true;
		else if (value < key) return left.includes(value);
		else if (value > key) return right.includes(value);
	}
}

public class NullNode extends Node {
	public boolean includes(int value) { return false; }
}
```
Lo schema del pattern è il seguente:
![[Pasted image 20250324084319.png]]
L'applicazione del pattern implica la definizione di una nuova versione della funzionalità `request()`, una versione "nulla". Oggetti nulli sono istanze di `NullObject`. Poiché abbiamo un oggetto e non un elemento puramente nullo, è necessario definire la funzione `request()` per stabilire il comportamento che deve avere l'oggetto quando l'oggetto non c'è. Questo permette anche di ridefinire il comportamento nel caso nullo semplicemente cambiando l'implementazione di `request()` nel `NullObject`, invece di modificare ogni riga di codice in cui abbiamo il caso `null`.
Ci sono tuttavia alcuni svantaggi nell'utilizzo di questo pattern:
- **forza uniformità**: clienti differenti potrebbero avere diverse idee sul "do nothing". Si potrebbero definire più `NullObject` in tal caso;
- **immutabilità**: oggetti `NullObject` non si possono trasformare in oggetti `RealObject`. Sarebbe utile avere un metodo che permette al `NullObject` di "trasformarsi" in un `RealObject`.
- **overhead**: l'aggiunta di `NullObject` implica occupazione in memoria. Tuttavia nel caso normale se il `NullObject` è di un solo tipo (quindi c'è solo un `NullObject`) e tale `NullObject` **non contiene stati**, non si ha la necessità di istanziare tanti oggetti nulli, bensì uno solo, e far si che ogni oggetto che dev'essere nullo punti a tale oggetto.
# Singleton
Come visto precedentemente, si può avere la necessità di avere un oggetto unico condiviso dalle entità del sistema. Per questo motivo si utilizza il pattern **Singleton**.
Non si tratta di avere un oggetto che abbia ogni attributo e metodo statico (l'istanza d'altronde non avrebbe senso), ma di avere un'unica istanza all'interno del programma. Infatti avere una variabile globale rende un oggetto accessibile, ma non lo rende obbligatoriamente unico. 
La soluzione risiede nel **vietare l'accesso al costruttore della classe**, rendendo il costruttore privato. Un metodo pubblico mi permette di ottenere l'istanza unica della classe: 
![[Pasted image 20250324091643.png]]
Come si nota, `uniqueInstance` è un'istanza di `Singleton` statica. Il metodo `getInstance()` restituisce `uniqueInstance`. Quando si restituisce `uniqueInstance`, bisogna assicurarsi che esso non sia nullo. Se esso non è nullo, semplicemente si restituisce il riferimento all'oggetto.
Nel caso del BST:
```java
class BSTNullNode {
	static final BSTNullNode instance;
    private BSTNullNode() {}
	static BSTNullNode getInstance() {
		if (instance == null) instance = new BSTNullNode();
		return instance;
	}
}
```
I vantaggi del pattern Singleton sono molteplici:
- la classe Singleton ha controllo di come e quando i client accedono alla classe;
- si evitano variabili globali che tengono la sola istanza condivisa;
- si può controllare il numero di istanze nel programma. Da notare che la logica di `getInstance()` può essere modificata per avere anche più di una sola istanza. Questo permette di gestire un **pool di istanze**;
- è un meccanismo più potente di `static`, permettendo di gestire anche più istanze come visto precedentemente.
