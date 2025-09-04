## Riusabilità tramite parametrizzazione
Risulta possibile riutilizzare un codice tramite la **parametrizzazione** delle variabili. Questo è possibile attraverso il meccanismo di **binding dinamico**, che permette a tempo di esecuzione di risolvere il tipo della variabile. 
In C++ è possibile fare ciò con i template:
```cpp
template <class T> class Stack {
	public:
		Stack(size_t size);
		~Stack();
		void push(const T &item);
		void pop(T &item);
		void top();
		bool isEmpty();
		bool isFull();
	private:
		size_t top;
		size_t size;
		T *_stack_;
};
```
A dover specificare il tipo `T` del modulo software riutilizzabile è il programmatore. Questo permette al software di poter avere più significati all'interno dell'applicazione
In Java l'allocazione in memoria di variabili di tipo `T` viene eseguita con un cast a `T` di variabili di tipo `Object`, in quanto ogni variabile allocata con `new` è internamente un puntatore ad un'area di memoria, che sarà quindi correttamente allocata.
## Glossario OOP
> *Un **ADT** è una specifica di proprietà, una regola a cui oggetti concreti potrebbero obbedire, e permette l'astrazione di molte entità del mondo reale.*
> *Un **oggetto** è un **modello** di un'unica entità della vita reale e un'istanza è la sua corrispondenza funzionante nel programma. Ogni istanza è unica e distinguibile dalle altre tramite la sua "identità".*
> *Un **modulo** è una collezione **unica** di programmi, usabile per realizzare un'istanza di un oggetto*.
> *Un'**istanza** è un programma allocato in memoria pronto per l'esecuzione. Sono anche chiamati **oggetti** in virtù del fatto che nel contesto del sistema software essi sono le rappresentazioni di questi.*
> *Una **classe** è un **artefatto sintattico** le cui semantiche possono essere un ADT, un oggetto, un modulo o una combinazione di queste. In Java, una classe è l'implementazione di un template per la creazione di istanze di oggetti.* 
> *Un **attributo di classe** è un identificatore statico allocato a tempo di compilazione memorizzato in una classe e condiviso tra tutte le istanze. L'istanza dell'oggetto a cui è riferito - che dev'essere statico - viene anch'esso condiviso.*
> *Un **attributo di istanza** è un identificatore dichiarato in una classe ma allocato a tempo di esecuzione nelle istanze di classe come copie separate.*
> *Un **metodo di classe** è un metodo globalmente disponibile per l'esecuzione, e viene chiamato dalla classe che lo definisce.*
> *Un **metodo di istanza** è un metodo di un oggetto che viene chiamato solo dall'istanza della classe che lo definisce.*
> *Un **call dispatch** è l'azione che consiste nel determinare quale codice eseguire in base al messaggio inviato ad un identificatore. Può essere fatto staticamente (**early binding**) con tipi statici associati, o dinamicamente (**late binding**) a tempo di esecuzione.*
> ***Self / this** sono alias utilizzati per fare riferimento all'istanza dell'oggetto che chiama il metodo.*
> ***Super** è un qualificatore di dispatch, **non** un identificatore. Indica di cercare un metodo corrispondente dalla classe genitore dell'oggetto considerato.*

Con "messaggio inviato ad un identificatore" e in generale con "scambio di messaggi" si intende la possibilità da parte degli oggetti di chiamare i metodi pubblici di altri oggetti.
Il call dispatch allora permette di risolvere la richiesta di esecuzione al metodo giusto. Ciò accade a tempo di esecuzione nel contesto del **polimorfismo**.
## Ereditarietà e polimorfismo
L'**ereditarietà** è il meccanismo utilizzato in fase di strutturazione del software che permette di derivare nuove classi a partire da quelle già definite realizzando una gerarchia di classi. L'ereditarietà è particolarmente potente, in quanto:
- permette di **riutilizzare il codice**, definendo quindi nuovi **sottotipi**;
- permette la scrittura di **codice generico**, tramite il meccanismo del **polimorfismo**.

Il **polimorfismo** è la proprietà della programmazione orientata agli oggetti in base alla quale oggetti di classi diverse possono rispondere in maniera diversa a una stessa invocazione. Supponiamo di avere due oggetti, uno di tipo `Cerchio` e l'altro di tipo `Quadrato`. Entrambe sono istanze di due classi che provengono dalla stessa classe `Forma`. Tramite il polimorfismo, il programma che scriviamo non deve obbligatoriamente conoscere il tipo di ogni forma, e può semplicemente chiamare la funzione `draw()` per entrambi i tipi: tramite il meccanismo di **late binding** il call dispatch permetterà di risolvere l'associazione codice-messaggio (intendendo "messaggio inviato all'identificatore" come spiegato precedentemente).