# Factory Method
All'interno dei nostri programmi dobbiamo utilizzare servizi forniti da altri agenti. Supponiamo di avere già l'implementazione di funzionalità, e lavoriamo sul lato client. Sappiamo esattamente quando è necessario istanziare un oggetto per utilizzare delle funzionalità di una classe (di cui sappiamo che è necessaria l'esistenza). Non vogliamo tuttavia istanziare esplicitamente la classe: in tal caso, infatti, dobbiamo sapere l'ADT di questo oggetto. Il problema quindi è il seguente: **sappiamo quando creare un oggetto di servizio, ma non sappiamo (o non vogliamo) stabilire quale classe istanziare**. Non vogliamo legare il codice client ad un ADT, in quanto tale legame è una dipendenza. Vogliamo rispettare il principio di programmazione secondo interfacce, in modo tale da rendere il programma client più riusabile, quindi **indipendente dal tipo di classe da utilizzare, pur sapendo di creare un'istanza di classe**.
Questo si differenzia dal **polimorfismo**. Un'istruzione è polimorfica quando lo stesso simbolo può indicare implementazioni diverse:
```java
public class Studente extends BasicStud {
	private List<Esame> esami = new ArrayList<>();
	public void nuovoEsame(String m, int v) {
		Esami e = new Esame(m, v);
		esami.add(e);
	}
}

public class Sospeso extends BasicStud {
	public void nuovoEsame(String m, int v) {
		System.out.println("Non possibile");
	}
}

public class Client {
	BasicStud s = new Studente();
	public void registra() {
		s.nuovoEsame("Maths", 8);
	}
}
```
L'istruzione `registra()` che crea una dipendenza con la classe specifica `Studente` si trova nel client. La classe client complessivamente ha una dipendenza: infatti `nuovoEsame()` è polimorfico, ma dipende dalla classe `Studente` e non dalla classe `BasicStud`.
## Soluzione
La soluzione che il pattern ci suggerisce consiste nello strutturare il programma in una maniera diversa. 
Consideriamo una classe astratta `Creator`, che costituisce il client: si tratta del modulo software in cui istanzieremo un oggetto di servizio. L'oggetto a cui faremo riferimento è un `Product`, pensato sempre come classe astratta. Il software è concepito come un client astratto che istanzia un prodotto astratto. 
In `Creator` il metodo `AnOperation()` fa riferimento alla presenza di un metodo che faccia uso dell'ADT astratto `Product`. Di tale ADT astratto dovrà esistere una implementazione; indichiamo tale implementazione come `ConcreteProduct`. Per far sì che il client resti indipendente da `ConcreteProduct`, utilizziamo un **`FactoryMethod()`**: tale metodo ha come scopo quella di creare l'oggetto di istanza, **nonché la scelta esplicita dell'istanza di oggetto da utilizzare**. 
In questo modo è possibile implementare un algoritmo `AnOperation()` che fa utilizzo di un oggetto, e nel momento in cui bisogna istanziare un prodotto assegniamo la responsabilità di tale operazione a `FactoryMethod()`.
![[Pasted image 20250331082611.png]]
Il `ConcreteCreator` avrà all'interno dell'implementazione di `FactoryMethod()` la creazione di un oggetto di istanza specifico. Il programma `Creator` quindi lavora dall'interfaccia astratta e il software diventa indipendente dall'ADT da utilizzare. Se in un secondo momento si inventa un nuovo `ConcreteProduct`, basta creare una nuova classe `ConcreteCreator` in modo tale che `FactoryMethod()` restituisca questo nuovo `ConcreteProduct` e il modulo software client è riutilizzabile.\
![Immagine pattern](<../!Images/Pasted image 20250331083019.png>)
### Implementazione esempio in Java
```java
public interface Product {
	void request();
}

class CProduct implements Product {
	public void request() {
		System.out.println("CProduct");
	}
}

abstract class Creator {
	abstract public Product getProduct();
	
	public void AnOperation() {
		Product p = this.getProduct();
		p.request();
	}
}

public class CCreator extends Creator {
	@Override
	public Product getProduct() {
		return new CProduct();
	}
}

public class Client {
	public static void main(String[] args) {
		Creator myApp = new CCreator(); // incapsulamento
		myApp.AnOperation(); // usa il servizio del prodotto in maniera indiretta
		
		Product myProduct = myApp.getProduct(); // incapsulamento
		myProduct.request(); // usa il servizio del prodotto in maniera diretta
	}
}
```
Con questo pattern abbiamo spostato la dipendenza dall'oggetto di servizio all'oggetto che ne incapsula la sua creazione. `CProduct` viene effettivamente utilizzato, ma in `Client` non si istanzia esplicitamente nessun `CProduct`. Questo crea quindi una **dipendenza indiretta** tra `Client` e `CProduct`: invece di istanziare direttamente l'oggetto `CProduct` nel `Client`, utilizziamo la classe `CCreator` come "tramite", rendendo il client **direttamente dipendente** a `CCreator`. 
## Utilità dello schema
Supponiamo di voler realizzare un'applicazione che tratti documenti, quindi un word processor. Vogliamo scrivere l'applicazione in modo tale che **supporti più tipologie di documenti**. I file quindi possono avere diversi formati, come file di testo, pdf e file word. Scriviamo allora una classe astratta `Application` che include i metodi `CreateDocument()`, `NewDocument()`, `OpenDocument()`. I metodi chiaramente saranno polimorfici in virtù del documento. La funzionalità di base è `CreateDocument()`. Possiamo strutturare quindi il software nel seguente modo:\
![alt text](<../!Images/Pasted image 20250331084216.png>)

Forniamo allora al nostro software un'indipendenza rispetto alla creazione di oggetti, creandoli senza fornire esplicitamente il tipo incapsulando l'istanziamento di oggetto.
Un'altra situazione in cui è utile il metodo è durante la creazione di **gerarchie di classi parallele**: \
![alt text](<../!Images/Pasted image 20250331084329.png>)\
La classe astratta `Figure` ha un metodo `CreateManipulator()` che restituisce un `Manipulator`, ulteriore classe astratta che fornisce metodi per manipolare la figura in virtù dell'implementazione. Le classi `LineFigure`, `TextFigure` sono figure di tipo diverso, in cui verranno implementate le funzioni `CreateManipulator()`. Invochiamo sugli identificatori quindi `CreateManipulator()` per ottenere un'istanza corretta di `CreateManipulator()` utilizzando tuttavia le interfacce `Figure`, `Manipulator`: il client quindi funziona con tutti i tipi di figure e tutti i tipi di manipolatore. Questa situazione è praticamente molto simile alla situazione dell'**Iterator Pattern**: il `CreateIterator()` è un factory method.
## Conseguenze del pattern
Il pattern permette di fornire indipendenza dall'ADT, fornendo possibilità di riutilizzare il software. Tuttavia, l'implementazione del `ConcreteCreator` dev'essere modificata per utilizzare altre funzionalità. Ne segue quindi che **il client è riutilizzabile, ma non le implementazioni dei creator**.\
Poiché per ogni `ConreteProduct` va derivato un nuovo `Creator`, si ha una **proliferazione di classi**.

## Creator come servente di client esterni
Il factory method può essere utilizzato per fornire un servizio di creazione di oggetti astratti:
```java
public class Creator {
	public IStudente getStudente() {
		return new Studente();
	}
}

public class SoCreator extends Creator {
	public IStudente getStudente() {
		return new Sospeso();
	}
}

public class Client {
	public void registra(Creator sc) {
		if (sc == null) sc = new SoCreator();
		//...
		IStudente s = sc.getStudente();
		s.nuovoEsame("Maths", 8);
	}
}
```
In questo caso in `Client` non si utilizza comunque il tipo esplicito dello studente, mentre `Creator` **invece di essere un tipo astratto è implementato come classe istanziabile**. La classe astratta quindi è stata collassata in una classe implementabile, fornendo comunque possibilità polimorfiche e astraendo la necessità di esplicitare il tipo di studente per il metodo `nuovoEsame()`. Se `Creator` viene passato come nell'esempio in maniera astratta il factory method presenta un enorme vantaggio.
## Variante: Creator generico
Perché funzioni il pattern è necessario creare una sottoclasse della classe `Factory`. Vi sono molti modi per implementare il Factory Method. La variante più simile all'implementazione classica consiste nell'utilizzo di **generics**. 
Una prima idea è quella di creare un software che funzioni con tanti tipi di prodotto: il prodotto che si utilizza è intercambiabile. Il software quindi è **parametrico** rispetto al prodotto. 
Allora potremmo creare una classe `Creator` astratta come segue:
```java
public interface Product {
	void request();	
}

public class AProduct implements Product {
	void request() {
		System.out.println("Request sent to AProduct");
	}
}

public class BProduct implements Product {
	void request() {
		System.out.println("Request sent to BProduct");
	}
}

public class CProduct implements Product {
	void request() {
		System.out.println("Request sent to CProduct");
	}
}

abstract class Creator<P extends Product> {
	abstract public P getProduct();
	
	public void AnOperation() {
		P p = this.getProduct();
		p.request();
	}
}

public class ConcreteCreator extends Creator<AProduct> {
	@Override
	public ProductA getProduct() {
		return new ProductA;

	}
	
}
```
Il vantaggio nell'utilizzo delle generics risiede nel fatto che il `ConcreteCreator` può essere definito come un tipo in modo tale che la visibilità del tipo è visibile al compilatore. Il tipo di prodotto scelto è visibile al compilatore. Quindi se volessimo un `ConcreteCreator` per il prodotto `BProduct`, sarebbe necessario creare un nuovo `ConcreteCreator` che estende `Creator<BProduct>`.

Potremmo tuttavia pensare di evitare di scrivere un metodo `getProduct()` astratto ed eseguire direttamente l'operazione:
```java
abstract class Creator<P extends Product> {
	public P getProduct() {
		return new P();
	}
	
}
```
Questa istruzione **non compila**, in quanto `P` non può essere risolto a tempo di compilazione. 

## Variante: factory method parametrico
Possiamo modificare il factory method e rendere esso parametrico. Se il factory method non dev'essere un metodo specifico ma incapsula una logica di scelta di prodotto, avere un unico `ConcreteCreator` con un `FactoryMethod()` parametrico può essere comodo. Tuttavia, avere un solo `Creator` può essere **limitante**. 

```java
public class StCreator {
	public static IStudente getStudente(boolean a) {
		if (a) return new Studente();
		return new Sospeso();
	}
}
```
Da questo momento in poi, tuttavia, il software può usare **un solo creator**. Se un domani volessimo creare un nuovo studente (assumendo in questo esempio vi sia solo `Sospeso` e `Studente`), è necessario modificare anche il client in quanto esso è dipendente dal `Creator`. Il numero di `Product` quindi **non è facilmente espandibile**.

## Variante: factory method statico
Un'altra alternativa consiste nel rendere il factory method **statico**. All'interno della classe `ConcreteCreator` è possibile definire attributi statici che ne definiscono lo stato. In questo modo:
- non è necessario istanziare il `ConcreteCreator`;
- non bisogna dichiarare alcun `Creator` astratto;
- **non è possibile eseguire override del factory method**. Questo può essere limitante in quanto i metodi statici non possono essere ridefiniti.

A livello implementativo allora implementeremo `StCreator` con le seguenti modifiche:
```java
public class StCreator {
	private static boolean a = true;
	public static IStudente getStudente() {
		if (a) return new Studente();
		return new Sospeso();
	}
}
```
Seguendo questa maniera le modifiche allo stato della classe `StCreator` vengono riflesse sui moduli e sugli oggetti che utilizzano `StCreator`.

In entrambi i casi (quindi sia factory method parametrico, sia factory method statico) la logica dell'istanziazione degli oggetti viene unificata in un unico metodo creazionale. Tale algoritmo creazionale viene condiviso in tutti i punti del software. 

## Variante: `Creator` con riflessione
Possiamo implementare un unico `Creator` utilizzando un parametro che permette di istanziare istanze del prodotto richiesto. La nostra classe `Creator` sarà la seguente:
```java
class Creator {
	Class<?> cp;
	public Creator(Class<?> cp) {
		this.cp = cp;
	}
	
	public Product getProduct() {
		if(this.cp != null) try {
			return (Product) this.cp.getConstructor().newInstance();
		}
		catch (InstantiationException | IllegalAccessException | IllegalArgumentException | InvocationTargetException | NoSuchMethodException | SecurityException e) {
			e.printStackTrace();
			return null;
		}
		return null;
	}
	//...

}

public class Client {
	public static void main(String[] args) {
		Creator myApp = new Creator(CProductA.class);

		Creator myOtherApp = new Creator(CProductB.class);
		//...
	}

}
```
Essenzialmente si controlla che tipo di classe `cp` identifica. Si estrae con `getConstructor()` il costruttore di `cp`, invocando il metodo per allocare un'istanza con `newInstance()`. Si tratta di un set di chiamate della libreria **Reflection API** di Java.

La Reflection API ispeziona a runtime un'istanza di una classe, deducendo il tipo di classe.

Risulta necessario ricordare che i generics sono simboli risolti a tempo di compilazione: si possono usare solamente a sinistra di un assegnamento. A tempo di esecuzione possiamo solamente verificare che il tipo a sinistra è compatibile con quello specificato a destra. Si tratta quindi di **etichette**, simili ai `#define` di C ma elaborate a tempo di compilazione; la reflection API invece controlla a tempo di esecuzione oggetti allocati in memoria.

Un approccio errato quindi che utilizza il Reflection API invece è il seguente:
```java
public class Client {
	public static void main(String[] args) {
		Creator<?> myApp = new Creator<CProductB>(); // specificato a compile-time
		//...
	}

}
//...
public class Creator<P extends Product> {
	P obj;
	Class<?> cp = obj.getClass();

	//...
}
```
Il codice crea un'eccezione, perché a tempo di esecuzione `obj` è un **oggetto nullo**, quindi il metodo `getClass()` causa una `NullPointerException`.


## Dependency Injection
Le implementazioni che abbiamo visto non utilizzano derivazioni, ma composizioni. Tramite un oggetto di un certo tipo si riesce ad incapsulare all'interno l'oggetto di servizio necessario. 

Il meccanismo utilizzato prende il nome di **dependency injection**. Supponiamo una classe `C` utilizzi un servizio fornito da `S` (quindi `C` **non** dipende da `S`). Esistono varie implementazioni di `S`, ad esempio `S1`, `S2`: vogliamo che `C` non dipenda dalle implementazioni `S1`, `S2`. Scriviamo la classe `C` affinché essa non sia dipendente dalle implementazioni. 

Facciamo allora in modo che `C` sia configurata con l'implementazione di `S` che si voglia utilizzare:
```java
	C myC = new C1(new S1());
```
La dipendenza viene così creata **a tempo di esecuzione**, rendendo la composizione di oggetti dinamica.
