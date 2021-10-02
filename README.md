# Sistemi Distribuiti M - 2021/2022

## 01.Modelli

###  (domanda provocatoria) Che cosa fa un laureato magistrale in ingegneria informatica in azienda oggi?
Comanda 😎

### Che cosa è un componente?
E' un "pezzo" di software che viene scritto dallo sviluppatore ed ha le seguenti caratteristiche:
- Contiene stato, metodi etc ma espone verso l'esterno solo quei metodi che si decidono che siano visibili all'esterno grazie all'uso di un'interfaccia;
- Viene eseguito all'interno di un container/engine/middleware.

Esempi di componenti sono quelli che si usano per creare le interfacce grafiche con **JavaFX**: textBox, label, comboBox etc. Quando si clicca su un bottone non si verifica se effettivamente il mouse è sopra al tasto e lo si schiaccia ma si scrive solo il codice che deve essere eseguito se quel determinato evento si verifica.

- **componente nel concentrato**: un componente che fa parte di un'applicazione che viene messa in esecuzione su una **sola** macchina
- **componente nel distribuito**: un concetto più ampio rispetto a quello del concentrato. Il componente non è vincolato a trovarsi su una sola macchina proprio per la definizione intrinseca di sistema distribuito. E' possibile spostarlo in qualsiasi momento da un nodo ad un altro. Per questo motivo il componente nel concentrato viene visto come se appartenesse ad un'applicazione "monolitica".

### Che differenza c'è tra un componente ed un oggetto?
**Uguale all'oggetto**:
- Mantiene dettagli di come è implementato: stato, metodi etc ed espone solo alcuni dei suoi dettagli tramite l'interfaccia

**Diverso dall'oggetto**:
- Il componente viene eseguito all'interno di un container/engine/middleware altrimenti non verrebbe eseguita la funzione di callback. Se si eseguisse il codice di un componente su una qualsiasi JVM non funzionerebbe. Riprendendo l'esempio di prima: chi è che controlla che effettivamente il mouse è posizionato sopra al bottone? Nessuno quindi il codice non potrebbe funzionare
- Il componente è di dimensioni più grande di un oggetto , in termini di codice, perchè il costo di overhead tra un'interazione e l'altra è maggiore. Ipotizziamo che due componenti A e B interagiscano tra di loro. A per comunicare con B deve instaurare una connessione, scambiare i dati e alla fine chiuderla. Nel concentrato, invece, gli oggetti anche se sono piccoli e interagiscono spesso fra di loro non hanno questo problema di overhead.

Ovviamente bisogna stare attenti a creare un componente non troppo grande per evitare di andare incontro a tutti quei problemi affrontati durante il corso di Ingegneria del Software T (riusabilità etc)

### Che tipi di modelli possiamo utilizzare?
Ogni problema presenta una soluzione diversa. Per capire meglio come risolverli è importante capire il funzionamento dei modelli.
I modelli che possono essere usati sono ad esempio:
- **statici/dinamici**: sicuramente un sistema dinamico consente di adeguarsi a fronte di variazioni mentre un sistema statico no
- **preventivi/reattivi**: un sistema preventivo è più costoso di un sistema reattivo perchè non è detto che un evento si verifichi. Ad esempio, i sistemi operativi non utilizzano sistemi preventivi. Se avviene un deadlock tra processi lo si sblocca dall’esterno tramite linea di comando.

E' meglio una soluzione statica o dinamica? Meglio una soluzione preventiva o reattiva? La risposta in generale che deve fornire un ingegnere è sempre: "dipende". Ogni problema ha una storia diversa. Questo perchè **non** esistono formule precise nei sistemi distribuiti dato che ci sono troppi parametri da prendere in considerazione: famiglia del processore, sistema operativo, linguaggio di programmazione etc.

### A cosa siamo interessati nei sistemi distribuiti?
Siamo interessati alle performance e ad eventuali colli di bottiglia. Per poterli evitare è necessario osservare le performance dell'applicazione e solo in seguito si capisce che cosa andare a modificare. Ad esempio, aprire troppe connessioni a un DB può introdurre un potenziale bottleneck.

Le modifiche non si effettuano modificando il codice stesso ma attraverso l'operazione di deployment (dispiegamento). Ad esempio, installare tutte le librerie necessarie che servono all'applicazione, copiare i file che devono essere locali all’applicazione, distribuire i componenti su uno o più nodi e mettere davanti un bilanciatore di carico etc.
Quando faccio deployment occorre decidere dove fare eseguire il componente e quali risorse ha bisogno per funzionare correttamente. Ad esempio, quando devo fare una Web App, ho un file che descrive queste scelte.

Come lo faccio? Ci sono diversi approcci:
- **Manuale**: l’utente determina ogni singolo oggetto/componente su quale è il nodo più appropriato
- **File Script**: si devono eseguire alcuni file di script che racchiudono la sequenza dei comandi per arrivare alla configurazione che presenta le dipendenze
- **Linguaggi dichiarativi**: supporto automatico alla configurazione attraverso linguaggi dichiarativi o modelli di funzionamento della configurazione da ottenere. Ad esempio, tramite il file di deployment XML e annotazioni.

### Quali sono le sfide dell’enterprise computing?
- Portabilità:
- Interoperabilità fra diversi ambienti (anche legacy)
- Supporto e gestione runtime: scalabilità, efficienza, tolleranza ai guasti, resilienza, affidabilità, qualità, etc
- Time-to-market: sviluppare componenti nel minor tempo possibile
- Integrazione

### Come sono evolute le architetture per le applicazioni enterprise?
Le architetture si sono evolute sempre di più verso architettura N-tier perchè l'obiettivo è quello di separare logicamente le funzionalità in modo da ridurre la complessità degli strati:
- **Single-Tier**: c'è un singolo calcolatore a cui sono connessi i clienti
    - **Vantaggi**: nessun aggiornamento dei clienti, una sola copia dei dati
    - **Svantaggi**: no scalabilità
- **Two-Tier**:
- **Three-Tier**:

### Cos’è J2EE?
E' un insieme di specifiche le cui implementazioni vengono principalmente sviluppate in linguaggio di programmazione Java e ampiamente utilizzata nella programmazione Web. Viene scritta solo la specifica non l’implementazione, diversi produttori di software hanno fatto diverse implementazioni; purché siano JEE compliant basta che rispettino la specifica. Alcune implementazioni si limitano alla specifica altre aggiungono funzionalità.

Esistono diversi software open source, che vengono spesso usati anche in ambiente di produzione:
- GlassFish: è l'implementazione di riferimento mantenuta da Oracle.
- WildFly: precedentemente noto come JBoss
- Apache TomEE: del quale esiste una versione avente supporto commerciale chiamata Tomitribe

JEE: edizione di Java standardizzata per supportare il modello componente container pesante che fa le operazioni di supporto. 

### Come funzionano i modelli a contenimento?

![zeus](./zeus.jpg)

---

## 02.EJB 2.x

### Che cos’è EJB?
E' una tecnologia a componenti server-side che consente di creare applicazioni di tipo enterprise.

Attualmente, non è in completamente in disuso ma si inserisce in un contesto in cui c'è ne sono moltre altre.

### Che caratteristiche offre?
multi-tier, transazionali, portabili, scalabili, sicure, ...

### Quali sono i principi di design di EJB?
- Le applicazioni EJB e i loro componenti devono essere debolmente accoppiati (loosely coupled). Ad esempio, se abbiamo due componenti A e B, A deve chiamare un metodo di B non molte volte. Questo perchè nel distribuito abbiamo un costo di overhead piuttosto alto. Dall'altra parte, i componenti devono essere portabili quindi bisogna stare attenti a come scrivere il software
- Il comportamento dei componenti EJB è definito tramite interfacce. Aspetto assolutamente non nuovo. Basta pensare agli oggetti.
- Lo sviluppatore **non** deve pensare a come gestione le risorse. Ci pensa tutto il container
- Le applicazioni EJB sono N-tier

### Che benefici offre EJB?
- Benefici del modello a componenti lato server
- Separazione fra logica di business e codice di sistema
- Framework di supporto per componenti portabili
- Supporto a facile configurazione a deployment-time

- Necessità di un tier con EJB per sfruttare funzionalità di middleware offerte dal container
Gestione risorse, gestione life-cycle delle istanze, controllo concorrenza e threading
Persistenza, transazioni e gestione della sicurezza
Scalabilità, affidabilità, disponibilità

### Che problemi ha avuto EJB 2.X?
- E' un container "pesante". Attualmente le tecnologie si sono spostate verso altre soluzioni
- Modello di programmazione non tanto simile a quello che si usa per sviluppare oggetti normali
- Difficoltà di testing(?) In che senso? EJB 2.x o proprio di EJB in generale?

### Quale è la sua architettura?
- **Logica di business**: scritta dallo sviluppatore
- **Runtime**: avrò sul nodo che ospita EJB Server non solo le istanze che il programmatore ha scritto ma anche altri due oggetti che vengono automaticamente generati:
    - **Oggetto EJB Home**: implementa l’interfaccia EJB Home. In terminologia J2EE si dice che il cliente implementa la Home Interface. È un proxy che intercetta la chiamata del cliente (la prima volta) e decide quale istanza logica gli deve restituire (una già creata, nuova…);
    - **Oggetto EJB Object**: implementa l’interfaccia EJB Object. In terminologia J2EE si dice che il cliente implementa la Remote Interface. È un proxy che ha la stessa interfaccia del componente EJB creato dallo sviluppatore. Quando invoco un metodo chiamo un EJB Object che invoca poi il Java Bean;

E' bene ricordare che la macchina server non è "pura" perché mi serve installarci anche iun container per far girare la mia applicazione

Pensiamo di sviluppare una Web App riguardante la banca dove un utente può solo prelevare e depositare soldi:
- Sviluppatore: creo solo la classe che chiamiamo Account. Non mi interessa niente riguardo le istanze, allocazione/deallocazione e thread. Scrivo il codice come se avessi solo un cliente come ci viene detto dalla specifica. A tutto questo ci pensa tutto il container;
- In EJB 2.x ad ogni classe creata, viene automaticamente generata una classe EJB Home e EJB Object perché così dice la specifica (contratto cliente);
- Cliente: C1, C2 e C3 richiedono il prelievo. Da specifica per conoscere EJB Home è importante che sia disponibile nel sistema dei nomi;
- C1 fa richiesta di prelievo e invoca su EJB Home create/find. Arriva a EJB Home (estensione di rmi.remote) che crea un oggetto O1 ed è l’istanza logica/concettuale dedicata per C1. EJB Home restituisce al cliente il riferimento di EJB Object;
- L’invocazione del metodo prelievo verrà fatta su EJB Object che a sua volta potrà invocare l’oggetto O1;
- C3 fa una richiesta. EJB Home (non c’è scritto da specifica se EJB Home è uno solo o di più) potrà creare un nuovo oggetto O2 oppure dare il riferimento di O1 (non c’è scritto da specifica).

### Quali contratti esistono?
Esistono due tipi di contratto:
- **Client view contract**: contratto tra cliente e container. Un contratto client view è costituito da:
    - Home interface:
    - Object interface:
    - Identità dell'oggetto? per arrivare alla home interface è necessario un servizio di nomi che mi consente di recuperare la home interface
- **Component contract**: contratto tra componente e container. Il contratto serve a gestire:
    - Abilita le invocazioni dei metodi dai clienti
    - Implementa le interfacce Home e Object per ridurre il carico di lavoro da parte dello sviluppatore
    - Gestisce la persistenza (solo in EJB 2.x, da EJB 3.x la gestione cambia)
    - Gestisce tutti i servizi di sistema: sicurezza, transazionalità etc.
    - Implementa il meccanismo delle callback. Ci sono alcuni Bean che vengono attivati quando si riceve un determinato messaggio

E' trasparente passare da EJBHome a EJBLocalHome? No, perchè non c'è la RemoteException

### Che funzionalità offre l’EJB container? Di cosa si occupa?
Le funzionalità che offre sono molteplici:
- Persistenza
- Transazionalità
- Gestione lifecycle componenti
- Connection pooling
- Threading
- Sicurezza

- Offre servizi di sistema
- Componenti EJB accedono risorse esterne (database, sistemi legacy, ...)
- La gestione delle risorse è compito del container, con obiettivi di massima efficienza

### Quali sono le tipologie di componenti Bean?
I Bean possono essere classificati in due categorie
- **Sincroni**: l'utente si blocca e aspetta la risposta da parte del server. Esistono due tipi di Bean:
    - **Session Bean**: a sua volta possiamo avere due tipi di Session Bean:
        - **Stateless**: il componente è privo di stato. Ad esempio, quando un'azione deve essere idempotente
        - **Statefull**: il componente è con stato. Ad esempio, quando si aggiungono i prodotti in un carrello di e-commerce
    - **Entity Bean**:
- **Asincroni**: l'utente non si blocca e non aspetta la risposta. Esiste un tipo di componente che fa parte di questa categoria:
    - **Message Driven Bean**:

### Cos’è un Session Bean? Quando va usato? Che tipi di Session Bean esistono?
- E' un componente che consente di effettuare calcoli computazionali.
- Una istanza per cliente
- Short-lived: vita del bean pari alla vita cliente
- Transient
- Non sopravvive a crash del server
- Può avere proprietà transazionali

### Cos’è un Entity Bean? Come si usano? Come può essere gestita la persistenza?
- Rappresenta dati di business
- Istanza condivisa fra clienti multipli
- Long-lived: vita del bean pari a quella dei dati nel database
- Persistente
- Sopravvive a crash del server
- Sempre transazionale

### Cosa sono e come funzionano i Message Driven Bean?
- Svolgono il ruolo di consumatori di messaggi asincroni

### Spiegare le interfacce EJBHome ed EJBObject


### Come agisce un cliente in EJB 2.X?


### Come avviene l’invocazione remota in EJB 2.X? Quali sono gli oggetti in gioco?

### Come funziona l’uso locale di EJB?

### Come avviene il deployment di una applicazione EJB?

IIOP protocollo comunicazione mondo corba
visione RMI del mondo CORBA
Suo standard legato al mondo CORBA

Problematiche:
costo unmarchaling/ + una chiamata gestione connessione (RMI bastato su IIOP)

Compile time?
si, interfaccia

Runtime?
Stub, skeleton

limite di RMI:
skeleton ha un'idea di ottimizzazione. Se è locale evito di comunicare remotamente

Standard no ma le implementazioni avevano già delle ottimizzazioni

## 03.Annotazioni

### Cosa sono le annotazioni? Esempi di annotazioni? Come sono strutturate?

Sono metadati con cui decoro i metodi, le classi, le interfacce etc. Non modificano il codice che lo sviluppatore scrive ma aggiungono solo informazioni che possono essere utili:
- Al compilatore
- Alla Javadoc
- A runtime.

Le annotazioni che sono state già viste sicuramente in altri corsi sono le seguenti:
- **@Overrided**: serve a livello di compilazione. Se non mettessi questa annotazione, il codice verrebbe generato lo stesso però devo stare attento a scrivere lo stesso nome del metodo sia nella classe padre che in quella figlia
- **@Deprecated**: serve a livello di documentazione per indicare che quel metodo è in disuso
- **@SuppressWarnings**: serve a livello di compilazione. Se la compilazione presenta dei warning questi vengono trascurati e non vengono mostrati all’utente.

Le annotazioni sono strutturate nel seguente modo: ci può essere solo il nome dell'annotazione o in caso ci fossero dei membri vengono scritti come un insiemi di coppie nome=valore. Se il membro è solo uno, il nome si può omettere.

Con *membro* si intende il "parametro di ingresso" dell'annotazione.

### Perchè si usano le annotazioni?

Le annotazioni consentono di arricchire lo spazio concettuale del linguaggio. Consente di fare programmazione dichiarativa oltre che a quella imperativa perchè consente di associare delle informazioni in modo dichiarativo al codice non andando a modificare il comportamento dei metodi, delle classi etc. Ad esempio, quello che viene specificato con il file di deployment si può fare benissimo tramite le annotazioni.

### Quali categorie di annotazioni esistono?

- **Marker annotation**: non hanno membri. Ad esempio: @Override
- **Single-value annotation**: hanno un solo membro. Ad esempio: @SuppressWarnings("unchecked")
- **Full annotation**: l'annotazione è formata da più di un membro
- **Custom annotation**: i programmatori possono crearsi le proprie annotazioni

### Che limiti hanno le annotazioni personalizzate?

- **Non** si possono avere relazioni di estensione (*extends*) fra tipi di annotazioni
- I tipi di ritorno degli eventuali metodi di una annotazioni devono essere: tipi primitivi, String, Class, enum, tipi di annotation o array dei tipi appena elencati
- Una annotation **non** può lanciare eccezioni ovvero non può avere una *throws clause*
- **Non** sono permessi *self-reference*. Ad esempio: AnnotationA non può contenere un membro di tipo AnnotationA
- **Non** sono permessi *circular-reference*. Ad esempio: AnnotationA non può contenere un membro di tipo AnnotationB e quest'ultimo di AnnotationA

### Cosa sono le meta-annotazioni? Quali sono?

Sono annotazioni che si specificano sui tipi personalizzati di annotazioni. Le meta-annotazioni sono:

- **@Target**: specifica il tipo di elemento al quale si può allegare tale tipo di annotazione (campo, metodo, classe, interfaccia etc.)
- **@Documented**: specifica che le annotazioni di tale tipo faranno parte della Javadoc
- **@Inherited**: questo tipo di annotazione funziona **solo** se apposta ad una classe. Il tipo di annotazione verrà automaticamente ereditato dalle sottoclassi della classe alla quale viene allegata
- **@Retention**: politica di mantenimento in memoria con cui il compilatore e JVM devono gestire le annotazioni

### Che valori possono avere le politiche di retention?

- **@Retention(RetentionPolicy.SOURCE)**: l'annotazione permane solo a livello di codice sorgente. Dunque, non viene memorizzata nel bytecode cioè nel file .class. Viene utilizzata solo a tempo di sviluppo da parte del compilatore. Ad esempio, l'annotazione @Override. Se confrontassi la dimensione del file in cui l'annotazione è stata scritta e quello in cui l'annotazione non è stata scritta, potremmo vedere che è la stessa
- **@Retention(RetentionPolicy.CLASS)(default)**: l'annotazione verrà registrata nel bytecode ma non verrà mantenuta dalla JVM a runtime. Dunque, non si può usare la reflection ma solo a tempo di caricamento. Ad esempio, si può decidere come trattare il caricamento tramite il class loader del bytecode ma poi le annotazioni non si possono sono usate a run-time
- **@Retention(RetentionPolicy.RUNTIME)**: l'annotazione verrà registrata nel bytecode e potrà essere letta a runtime tramite reflection anche dopo il caricamento della classe da parte della JVM. E' utilizzabile anche all’interno del codice di supporto/applicativo a tempo di esecuzione, con proprietà eventualmente modificabili a runtime

## 04-

Sistema di nomi: RMI Registry (RMI), DNS, Port Mapper (RPC)

Nome logico, numero di programma, numero di protocollo -> porta

Discovery: es. bluetooth protocollo di discovery. mando dei broadcast ma non so se gli altri dispositivi hanno il bluetooth

Directory: LDAP, es. accesso ai laboratori di UNIBO

```
@Target ( {ElementType.METHOD,ElementType.PACKAGE} )
public @interface ExampleAnnotation { ... }
```