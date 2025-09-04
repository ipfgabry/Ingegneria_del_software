# Caratteristiche della OOP
Ricordiamo le tre principali caratteristiche della **Object Oriented Programming**:
- **incapsulamento**, ossia la proprietà degli oggetti di **incorporare** al loro interno attributi e metodi;
- **ereditarietà**, ossia la capacità di costruire nuove classi a partire da classi già sviluppate;
- **polimorfismo**, ossia la possibilità per i metodi di assumere implementazioni diverse utilizzando tuttavia **lo stesso identificatore**.

A livello sintattico viene aggiunto solamente un solo costrutto, ossia il costrutto **`class`**. 
La OOP risolve problemi quali la **namespace pollution** e la **non-rientranza**:
- la prima fa riferimento all'impossibilità di poter utilizzare un nome in quanto lo stesso simbolo è già utilizzato nello stesso namespace (o header file, ad esempio, nel caso di un linguaggio strutturato come C);
- la seconda fa riferimento all'impossibilità di invocare in maniera sicura una funzione prima che una sua precedente invocazione è stata completata (id est può essere eseguita in maniera concorrente). Questo accade ad esempio quando una funzione lavora su variabili che sono globali e quindi esterne alla funzione.

Ciò viene raggiunto tramite l'aggiunta di gradi di **astrazione**. Nel paradigma di programmazione strutturata o procedurale possiamo raggiungere ciò definendo degli **Abstract Data Type** (**ADT**). L'aggiunta di gradi di astrazione permette di poter **riutilizzare il codice**. Lo stesso principio vale nella programmazione a oggetti. La programmazione strutturata tuttavia non permette un livello sufficiente di astrazione:
1. non abbiamo alcuna garanzia di inizializzazione e di terminazione;
2. non si può generalizzare la gestione degli errori;
3. il compilatore (o interprete) non **forza** l'occultamento dei dati.

Ad esempio, nel linguaggio C possiamo accedere direttamente ai dati contenuti all'interno di una struct in maniera del tutto legale (ossia senza che il compilatore lo impedisca). Quindi è concreta la possibilità di scrivere codice che può generare errore con più facilità.
La soluzione a questi problemi risiede nell'utilizzo delle **classi**, un tipo di dato astratto che permette di incapsulare e nascondere informazioni. Ogni classe contiene un **costruttore** e un **distruttore** per inizializzare e de-allocare gli oggetti in maniera automatica (in C++ dipende se l'oggetto è allocato dinamicamente. La chiamata al distruttore tuttavia è automatica quando viene usato `delete`).

## Gestione delle eccezioni
Un'**eccezione** è un nome che diamo ad una situazione di errore relativo ad un concetto. Non si tratta di un errore di sistema.
A livello implementativo un'eccezione non è altro che una **classe** che gestisce una problematica. Tale problematica è relativa ad un ADT e non sono **generiche**, ma si ripresentano nel contesto dell'ADT. 
L'eccezione permette di definire l'errore in fase di progettazione e la gestione di questo nell'implementazione del client. Quindi permette di disaccoppiare l'identificazione dell'errore dalla sua gestione.
In C++ è possibile provocare un'eccezione con `throw`:
```cpp
//...
void foo() {
	if ({condition}) {
		throw {exception}
	}
}
//...
```
In questo modo possiamo gestire i casi sia comuni sia particolari senza scrivere blocchi di codice che **prevedono** le eccezioni durante le operazioni:
```cpp
try {
	//...
}
catch (Exception Ex) {
	// gestisci eccezione
}
```
Il sollevamento delle eccezioni in Java va specificato nella firma della funzione:
```java
public void pop(T item) throws StackUnderflowException {
	if (this.is_empty()) throw StackUnderflowException(this);
	//...
}
```