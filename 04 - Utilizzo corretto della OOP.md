# Utilizzo corretto della OOP: esempio pratico
## Requisiti
Consideriamo un componente piccolo che dati vari file contenenti valori numerici, con un valore per ciascuna riga del file:
1. leggere da ciascun file la lista di valori;
2. tenere solo i valori non duplicati;
3. calcolare la somma dei numeri letti dal file (non duplicati);
4. calcolare il massimo tra i numeri letti.

Quando il software sarà pronto, saranno questi requisiti che dovranno essere soddisfatti dal risultato della programmazione.
## Sviluppo 
Per prima cosa avremo bisogno di importare alcune librerie. In particolare, importeremo:
```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.io.LineNumberReader;
import java.util.ArrayList;
import java.util.List;

```

Importiamo:
- la classe `File`, che permette di manipolare file o in generale stream di input e output (?);
- la classe `FileReader`, che permette di fornire operazioni di lettura più potenti per file;
- la classe `IOException`, che permette di gestire le eccezioni di lettura e scrittura (ad esempio, quando un file su disco non esiste o non è pronto per la lettura/scrittura);
- la classe `LineNumberReader`, un altro reader per la lettura della riga del file;
- la classe `ArrayList` e la classe `List`.

L'idea di importare queste classi è quella di creare strutture più avanzate a partire da classi più semplici. A partire dalla classe `File`, si può istanziare la classe `FileReader`. A partire da `FileReader`, si può istanziare la classe `LineNumberReader` .
Abbiamo utilizzato il termine "classe", ma a dir la verità abbiamo utilizzato dei nomi, che potrebbero indicare sia classi sia interfacce. Infatti, `ArrayList` è una **classe**, ma `List` è una **interfaccia**. 
Così come esiste il `FileReader`, esiste un `FileWriter`, istanziabile come segue:
```java
	FileWriter F = new FileWriter("test.txt", true);
```
La variabile booleana indica le modalità di append o overwrite:
- se la booleana è `true`, siamo in modalità append;
- se la modalità è `false`, il testo verrà sovrascritto.
#### Collezione
L'astrazione principale per gli aggregati di oggetti è la **collezione**, un concetto che indica un aggregato generico di elementi che permette la navigazione di questi. In Java è stato introdotto il concetto di collection come astrazione di un generico aggregato. Nel nostro caso, tale collezione è una **lista**, un aggregato sequenziale di elementi. Gli elementi non sono obbligatoriamente ordinati in maniera sequenziale tra loro.
Nel nostro caso, la lista è un array di oggetti di tipo `T`. Nel momento in cui `ArrayList` è vuoto, i valori degli oggetti di tipo `T` sono nulli: gli elementi saranno aggiunti o tolti uno dopo l'altro. A livello astratto essa è una sequenza di elementi che può essere anche implementata con una lista concatenata. 

### Definizione della classe
Definiamo ora la classe `CalcolaImporti`:
```java
public class CalcolaImporti{
	private final List<String> importi = new ArrayList<>();
```
La lista è una struttura dati sequenziale e omogenea generica. `ArrayList` permette di implementare la lista attraverso un array.
Dichiariamo ora la variabile `totale` e la funzione `calcola`:
```java
	//...
	private float totale;
	public float calcola(String c, String n) throws IOException {
		LineNumberReader f = new LineNumberReader(new FileReader(new File(c, n)));
		
	}
```
`LineNumberReader` è una classe che è costruita sulla classe più semplice `FileReader`, a sua volta istanziata sulla base della classe più semplice `File`, che presenta due parametri: il nome del file e il prefisso, ossia il percorso per tale file. Nel caso in cui il file si trovi nella stessa directory, allora basta scrivere `.`.
#### Composizione e derivazione
Il `FileReader` è stato costruito sulla base della classe `File`. Più precisamente, possiamo dire che l'oggetto di tipo `FileReader` è stato costruito con una **composizione** con la classe `File`. Ossia, non abbiamo detto che `FileReader` eredita da `File`, ma abbiamo istanziato `File` e `FileReader` **ha** un `File`, ma **non è** un `File`. La composizione di conseguenza è un **legame dinamico**, mentre l'ereditarietà è un **legame statico** in quanto è un **legame di tipo** che definisce la struttura del software, immutabile durante l'esecuzione. Lo stesso può essere detto per il `LineNumberReader`: `LineNumberReader` aggiunge una funzionalità a `FileReader`, ossia la possibilità di calcolare il numero della riga letta.
La composizione permette di creare funzionalità più avanzate senza derivare caratteristiche da una classe base.
### Ciclo di lettura
Continuiamo con:
```java
	//...
	totale = 0;
	while (true) {
		final String riga = f.readLine();
		if (riga == null) break;
		importi.add(riga);
		totale += Float.parseFloat(riga);
	}
	f.close();
	return totale;
```
`add()` è un metodo dell'[[01 - Java - Prima Lezione#Ereditarietà e interfacce|interfaccia]]`List`. 
`parseFloat()` è un metodo della classe `Float` (quindi `static`).
`f.close()` è necessario per chiudere il file. Il bytecode verifier **non** notificherà che `f.close()` non è stato invocato.
#### Precisazione su `final`
Porre `final` su una classe indica che **da quella classe non si possono derivare altre classi**. Se un metodo è posto come `final`, una sottoclasse **non può eseguire un override del metodo**. Nel nostro contesto, `final String riga` indica semplicemente che `riga` è costante.
#### Precisazioni sulla tipologia dello sviluppo dell'esempio
Quello che abbiamo fatto per ora è una bozza del nostro software utilizzando un approccio **incrementale**, ossia aggiungendo di volta in volta funzionalità. Ora come ora manca:
1. l'implementazione del massimo;
2. l'eliminazione dei duplicati.
### Codice finale
Aggiungiamo le implementazioni finali e si ottiene il codice:
```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.io.LineNumberReader;
import java.util.ArrayList;
import java.util.List;

public class CalcolaImporti{
    private final List<String> importi = new ArrayList<>();
    private float totale, massimo;
    public float calcola(String c, String n) throws IOException {
        LineNumberReader f = new LineNumberReader(new FileReader(new File(c, n)));
        totale = massimo = 0;
        while(true) {
            final String riga = f.readLine();
            if (riga == null) break;
            if(!importi.contains(riga)) {
	            importi.add(riga);
	            float x = Float.parseFloat(riga);
	            totale += x;
	            if (massimo < x) massimo = x;
	        }
        }
        f.close();
        return totale;
    }
}
```
L'algoritmo tuttavia **non è conforme ai principi Object-Oriented**. Infatti vengono eseguite nello stesso algoritmo eseguo:
- lettura del file;
- inserimento delle righe lette;
- calcolo della somma;
- calcolo del massimo;
- restituzione della somma.

Questo modo di procedere prende il nome di **anti-pattern**, perché è un approccio da evitare (o spaghetti code). Ci sono alcune indicazioni che il nostro codice non segue un corretto pattern:
- i metodi sono **lunghi**, senza parametri e usano variabili globali;
- viene ignorato il principio di **singola responsabilità**, ossia il metodo svolge più compiti assieme. Una funzionalità dev'essere la più piccola possibile;
- il flusso di esecuzione è determinato solamente dall'implementazione dell'oggetto. Ossia, l'ordine di esecuzione è **cablato** in un'implementazione monolitica dell'oggetto;
- ereditarietà e polimorfismo non possono essere utilizzate. Il metodo vecchio, infatti, dev'essere riutilizzato in blocco, o se dev'essere ridefinito, va ridefinito l'intero blocco;
- il nome stesso della classe tradisce una cattiva implementazione dell'ADT. Nel nostro caso infatti coincide con la funzionalità che abbiamo dentro. Non abbiamo definito un ADT, ma un algoritmo monolitico racchiuso dentro una classe.

Cerchiamo allora di dividere i compiti:
```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.io.LineNumberReader;
import java.util.ArrayList;
import java.util.List;

public class CalcolaImporti{
    private final List<String> importi = new ArrayList<>();
    private float totale, massimo;
    public float calcola(String c, String n) throws IOException {
        LineNumberReader f = new LineNumberReader(new FileReader(new File(c, n)));
		String riga;
        while(true) {
            riga = f.readLine();
            if (riga == null) break;
            if(!importi.contains(riga)) importi.add(riga);
	    }
	    f.close();

		totale = 0;
		for(int i = 0; i < importi.size(), i++) {
			totale += Float.parseFloat(importi.get(i));
		}

		massimo = Float.parseFloat(importi.get(0));
		for (int i = 1; i < importi.size(); i++) {
			if (massimo < Float.parseFloat(importi.get(i)))
				massimo = Float.parseFloat(importi.get(i));
		}
		return totale;
	}
}
```
In questo modo abbiamo separato i compiti. Possiamo adesso separare il calcolo di queste funzioni e definire un ADT `Pagamenti`:
```java
public class Pagamenti {
	private List<String> importi = new ArrayList<>();
	private float totale, massimo;
	
	public void leggiFile(String c, String n) throws IOException {
		LineNumberReader f = new LineNumberReader(new FileReader(new File(c, n)));
		String riga;
        while(true) {
            riga = f.readLine();
            if (riga == null) break;
            inserisci(riga);
	    }
	    f.close();
	}
	
	public void inserisci(String riga) {
		if(!importi.contains(riga)) importi.add(riga);
	}
	
	public void calcolaSomma() {
		for (String v : importi)
			totale += Float.parseFloat(v);
	}

	public void calcolaMassimo() {
		for(String v : importi) 
			if (massimo < Float.parseFloat(v))
				massimo = Float.parseFloat(v);
	}

	public void svuota() {
		importi = new ArrayList<>();
		totale = massimo = 0;
	}

	public float getMassimo() { return massimo; }
	public float getSomma() { return totale; }
}
```
La classe risulta essere più riutilizzabile, sebbene sia più costosa in quanto il calcolo della somma, del massimo e della lettura del file sono tutte chiamate a funzione separate. 
La scrittura `for (String v : importi)` è un **enhanced `for`** che permette di navigare gli elementi presenti in un aggregato generico. Questo tipo di ciclo `for` è valido per ogni oggetto `Iterable` ed è pensato per risolvere il problema dell'iterazione sugli elementi.
Le metriche per misurare un software generalmente sono:
- numero di classi;
- numero di metodi;
- numero di righe di codice (LOC, Lines Of Code).

Tali metriche sono utili per verificare se sono stati applicati correttamente i principi della OOP.
### Command and Query
In questo caso stiamo utilizzando un paradigma noto come paradigma **Command and Query**: ogni metodo modifica lo stato dell'oggetto e non ritorna nessun valore (command) oppure non modifica lo stato dell'oggetto ma restituisce un valore interrogando l'oggetto per vedere qual è il suo stato (query).
## Convalida (Testing)
Per la convalida basta dichiarare alcune funzioni di testing, come `testLeggiFile()`, `testCalcolaMassimo()` e `testCalcolaSomma()`, e richiamarle nel metodo `main()`.
Importante è anche avere un metodo che porta il mio sistema in uno stato necessario per il corretto funzionamento del nostro programma. 
Per fare testing è importante anche avere un **oracolo**, ossia un qualcosa che permetta di verificare che l'output sia corretto. Tale oracolo è derivabile manualmente dai requisiti.
Ci sono strumenti più avanzati per eseguire testing del software prodotto.
