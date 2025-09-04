Java è un linguaggio di programmazione strutturato che segue il paradigma della programmazione orientata ad oggetti. Java inoltre è **puramente ad oggetti**, pertanto ogni programma richiede la definizione di una classe.

Java non produce un eseguibile tramite un processo diretto di compilazione come in linguaggi quali C++, dove il pre-processore risolve le direttive, il compilatore traduce il sorgente in linguaggio assembly e un assembler produce uno o più file oggetto che tramite il linker vengono uniti in un unico eseguibile. Il sorgente di Java viene tradotto in un **bytecode** dal compilatore Java, che è un linguaggio assembler specifico per un processore virtuale, la **Java Virtual Machine** (**JVM**). Utilizzando la JVM si potrà poi tradurre il linguaggio nel linguaggio assembler per lo specifico calcolatore. Si tratta quindi di un processo di compilazione in un linguaggio bytecode che poi verrà tradotto in linguaggio macchina localmente. Questo permette di produrre un eseguibile che viene compilato da qualsiasi macchina a patto che la macchina sia dotata del processore virtuale JVM.
Ci sono 3 vantaggi principali in questo processo di compilazione:
- **portabilità**: il bytecode di Java permette al codice di essere compilato su qualsiasi piattaforma con una JVM compatibile;
- **sicurezza**: nel processo di compilazione si ha una verifica del bytecode che impedisce la compromissione del sistema da parte di un codice dannoso;
- **efficienza**: il bytecode viene effettivamente compilato in porzioni.

 In Java inoltre esiste il **garbage collector**, che al termine dell'esecuzione del programma libera la memoria occupata dagli oggetti allocati durante l'esecuzione. 
### Processo di compilazione di un file Java
L'ambiente di esecuzione per i programmi Java è costituito da due blocchi principali:
- la **macchina virtuale Java** (in inglese **JVM, Java Virtual Machine**);
- le **API** Java.

La macchina virtuale è la base della piattaforma Java, mentre le API sono le librerie usate dal codice Java. 
I file sorgenti Java vengono memorizzati come file `.java`. Il compilatore Java (**`javac`**) genera il bytecode, traducendo il codice sorgente in una rappresentazione intermedia indipendente dalla piattaforma, verificando che non ci siano errori sintattici. Una volta compilato, il bytecode verrà fornito alla JVM per l'esecuzione. Da notare che il bytecode presenta istruzioni utilizzando un linguaggio **astratto**, del tutto **indipendente dal sistema su cui verrà eseguita l'applicazione**. 
Dopo la compilazione del bytecode, la JVM si occupa di **virtualizzare** l'hardware su cui viene eseguito il risultato della compilazione. La JVM è costituita da tre elementi:
1. il **class loader**;
2. il **bytecode verifier**;
3. l'**interprete**.

Il **class loader** carica le classi che formano il bytecode. Dopo che le classi vengono caricate, il **bytecode verifier** controlla che il bytecode sia valido, verificando che non ci siano operazioni che portano a stati illegali della JVM (ad esempio, operazioni che portano a **violazioni di memoria**, spesso verificando che non ci siano operazioni di aritmetica dei puntatori). Se il bytecode passa i controlli, allora può essere eseguito dall'interprete. 
L'interprete può essere a sua volta di varie forme:
1. può essere un normale interprete, che esegue uno per uno ogni istruzione del bytecode;
2. può utilizzare un **JIT compiler**, ossia un **compilatore just-in-time**, che traduce il bytecode in codice macchina del calcolatore su cui viene eseguito il programma. 

Esiste una soluzione mista, in cui la compilazione JIT avviene solo per le porzioni di codice più usate, mentre il resto viene interpretato. 
## Struttura del sorgente Java
Ogni programma Java deve contenere una sola classe. Ogni file Java deve avere un solo metodo `main`. Da notare che ogni software Java può contenere più `main` possibili, in quanto esso è definibile in ogni classe del software.
Il main viene dichiarato **sempre** come:
```java
	public static void main(String[] args) {
	...
	}
```
Il parametro `String[] args` indica i possibili argomenti che vengono passati nel momento dell'esecuzione del file. Il file sorgente viene memorizzato con estensione `.java`. Il sorgente viene compilato a riga di comando con:
```bash
	:~$ javac <nome_file>.java
```
Producendo un file con estensione `.class`. Il compilatore `javac` fa riferimento ad una variabile di sistema nota come **`javaclasspath`**, che indica dove sono localizzate le classi standard che possono essere importante nel codice. Possiamo aggiungere ulteriori `classpath` utilizzando l'opzione `-cp`:
```bash
	:~$ javac -cp <path> <nome_file>.java
```
Per eseguirlo basta utilizzare:
```bash
	:~$ java <nome_file>
```
Senza scrivere l'estensione.
Le librerie possono essere importate usando `import`, come segue:
```java
import java.time.LocalDate
```
che importa da `time` il tipo `LocalDate`. Java organizza le sue classi in una struttura gerarchica organizzata in **folder**: questi folder prendono il nome di **package**. Lo snippet di codice soprastante quindi indica di utilizzare la classe `LocalDate` che si trova dentro il package `time` che si trova dentro il package `java`: poiché questi sono folder, è come se stessimo indicando un path al file `LocalDate.class`. 
Per creare una classe all'interno di un nuovo package, è necessario specificare nel programma:
```java
package <folder_name>
```
dove `<folder_name>` specifica la cartella in cui è definito il file. Per eseguire con `java`, dobbiamo andare nel folder principale ed eseguire:
```bash
	:~$ java <folder_name>.<nome_file>
```
Il folder principale nel nostro caso è la home stile UNIX `~/`. All'interno della home ci sarà un folder `<folder_name>`.
In Java inoltre **non sono presenti puntatori**. 
Per stampare in java, si utilizza:
```java
System.out.println("stringa esempio");
```
Per leggere invece input dall'utente, si utilizza la classe *Scanner*:
```java
import java.util.Scanner

Scanner input = new Scanner(System.in);
```
Viene così istanziato un oggetto di tipo *Scanner* a cui viene passato lo stream da cui prendere l'input. Un programma esempio è il seguente:
```java
import java.util.Scanner;

public class ScannerDemo {
	public static void main(String[] args) {
		Scanner input = new Scanner(System.in);

		System.out.println("Inserisci nome:" );
		String name = input.nextLine();
		
		System.out.println("Ciao " + name);
	}
}
```
### Allocazione delle variabili in Java
Le variabili che non hanno un tipo primitivo sono tutte di tipo *oggetto*. Il nome della variabile in questo caso è un **riferimento all'oggetto** che punta all'oggetto allocato in memoria.
Di conseguenza il fatto che in Java non sono presenti puntatori fa riferimento alla impossibilità di utilizzare puntatori in maniera libera come in C++. 
Ad esempio:
```java
	String nome = "Paolo";
```
indica che `nome` è una variabile che è un riferimento ad un oggetto che punta all'area di memoria istanziata per l'oggetto. 
### Confronto tra oggetti
Come in C e C++, se vogliamo verificare che due oggetti sono uguali, ad esempio due stringhe, non possiamo semplicemente usare l'operando `==`, ma si confrontano i contenuti degli oggetti. In particolare, per due stringhe `s1, s2` si utilizza `s1.equals(s2)`. 
### Confronto con C++
Per fare un confronto con C++, vediamo adesso alcune differenze di sintassi: 

| C/C++                                                     | Java                                             |
| --------------------------------------------------------- | ------------------------------------------------ |
| `T a;` alloca una struct di tipo `T`                      | `T a;` alloca l'id `a` di tipo `T`               |
| `a++`; aumenta di uno `a` (può anche essere un puntatore) | `a++`; illegale                                  |
| `a->m();` accede al metodo `m()` del puntatore `a`        | `a.m();` accede al metodo `m()` dell'oggetto `a` |
Nella definizione delle classi in Java si pone sia l'astrazione che la sua implementazione. Questo è un obbligo che non è presente in C++: infatti si possono dichiarare i prototipi e poi definire le funzioni in un secondo momento. 
I tipi ai lati di un assegnamento devono essere formalmente compatibili a tempo di compilazione. A tempo di esecuzione si verifica che i puntatori sono compatibili e che il tipo assegnato al puntatore sia compatibile al puntatore stesso.
### Ereditarietà e interfacce
A differenza di C++, Java **non** supporta l'ereditarietà multipla. Questo vincolo permette di avere gerarchie lineari di classi. Risulta possibile "aggirare" questo vincolo utilizzando le **interfacce**.
Il problema dell'eredità multipla risiede nel fatto che concettualmente, se `A` e `B` sono due tipi distinti e rappresentabili come due insiemi disgiunti, dire che una classe `C` eredita sia da `A` sia da `B` è una contraddizione: sarebbe come definire un sottoinsieme comune ad `A` e a `B`. Definire un'interfaccia è simile a definire una classe astratta, ma ci sono delle differenze:
1. non sono presenti attributi non `final` nell'interfaccia;
2. è possibile implementare metodi all'interno dell'interfaccia, a differenza delle classi astratte, che non prevedono l'implementazione dei metodi dichiarati;

L'interfaccia, inoltre, **non** rappresenta un ADT, ma un **gruppo di funzionalità affini tra loro**, che è un concetto meno significativo di ciò che definisce un tipo di entità. Rappresenta quindi un modo d'uso, comune a più tipologie di entità anche non correlate tra loro. Ha senso ora concettualmente pensare che per un oggetto si possano implementare più interfacce.
Si può anche simulare l'ereditarietà multipla con il **pattern decorator** se esso è implementato correttamente.
### Keyword `Static`
La keyword `static` permette di produrre un blocco di codice unico immediatamente disponibile per l'esecuzione. Si può quindi avere codice staticamente eseguibile **anche se non ci sono oggetti istanziati**. 
Un metodo che viene dichiarato `static` diventa **unico** e **globale**, quindi non ci potranno essere altri metodi con lo stesso nome nell'applicazione. `static` pertanto permette di definire variabili e metodi globali.
### Keyword `final`
La keyword `final` indica che la variabile non può essere riassegnata, quindi assume lo stesso significato di `const` in C++.