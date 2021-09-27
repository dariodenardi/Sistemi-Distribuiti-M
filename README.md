# Sistemi Distribuiti M - 2021/2022

## 01-Modelli

### Che cosa è un componente?
E' un "pezzo" di software che viene scritto dallo sviluppatore ed ha le seguenti caratteristiche:
- Contiene stato, metodi etc ma espone verso l'esterno solo quei metodi che si decidono che siano visibili all'esterno grazie all'uso di un'interfaccia;
- Viene eseguito all'interno di un container/engine/middleware.

Esempi di componenti sono quelli che si usano per creare le interfacce grafiche con JavaFX: textBox, label, comboBox etc. Quando si clicca su un bottone non si verifica se effettivamente il mouse è sopra al tasto e lo si schiaccia ma si scrive solo il codice che deve essere eseguito se quel determinato evento si verifica.

- **componente nel concentrato**: un componente che fa parte di un'applicazione che viene messa in esecuzione su una **sola** macchina
- **componente nel distribuito**: un concetto più ampio rispetto a quello del concentrato. Il componente non è vincolato a trovarsi su una sola macchina proprio per la definizione intrinseca di sistema distribuito. E' possibile spostarlo in qualsiasi momento da un nodo ad un altro. Per questo motivo il componente nel concentrato viene visto appartenente ad un'applicazione monolitica.

### Che differenza c'è tra un componente ed un oggetto?
**Uguale all'oggetto**:
- Mantiene dettagli di come è implementato: stato, metodi ed espone solo alcuni dei suoi dettagli tramite l'interfaccia

**Diverso dall'oggetto**:
- Il componente viene eseguito all'interno di un containercontainer/engine/middleware altrimenti non verrebbe eseguita la funzione di callback. Se eseguissi il codice di un componente invocando una JVM non funzionerebbe. Riprendendo l'esempio di prima: chi è che controlla che effettivamente il mouse è sopra al tasto?
- Ha ***grana grossa***: il componente è di dimensioni più grandi di un oggetto perchè il costo di overhead tra un'interazione e l'altra è maggiore. Ipotizziamo che due componenti A e B interagiscano tra di loro. A per comunicare con B deve instaurare una connessione, scambiare i dati e alla fine chiuderla. Nel concentrato, invece, gli oggetti anche se sono piccoli e interagiscono spesso fra di loro non hanno questo problema di overhead.

Ovviamente bisogna stare attenti a creare un componente non troppo grande per evitare di andare incontro a tutti quei problemi affrontati durante il corso di Ingegneria del Software T (riusabilità etc)

### A cosa siamo interessati nei sistemi distribuiti?
Siamo interessati alle performance e ad eventuali colli di bottiglia. Per poterli evitare è necessario osservare le performance dell'applicazione e solo in seguito si capisce che cosa andare a modificare.

Le modifiche non si effettuano all’interno del codice stesso ma attraverso l'operazione di deployment (dispiegamento). Ad esempio, installare tutte le librerie necessarie che servono all'applicazione, copiare i file che devono essere locali all’applicazione, distribuire i componenti su uno o più nodi e mettere davanti un bilanciatore di carico etc.
Quando faccio deployment occorre decidere dove fare eseguire il componente e quali risorse ha bisogno per funzionare correttamente. Ad esempio, quando devo fare una Web App, ho un file che descrive queste scelte.

Come lo faccio? Ci sono diversi approcci:
- **Manuale**: l’utente determina ogni singolo oggetto/componente quale è il nodo appropriato
- **File Script**: si devono eseguire alcuni file di script racchiudono la sequenza dei comandi per arrivare alla configurazione che presenta le dipendenze
- **Linguaggi dichiarativi**: supporto automatico alla configurazione attraverso linguaggi dichiarativi o modelli di funzionamento della configurazione da ottenere. Ad esempio, tramite il file di deployment XML e annotazioni.

### Che tipi di modelli possiamo utilizzare?
Ogni problema presenta una soluzione diversa. Per capire meglio come risolverlo è importante capire il funzionamento dei modelli.
I modelli che possono essere usati sono:
- statici/dinamici
- preventivi/reattivi

E' meglio una soluzione statica o dinamica? Meglio una soluzione preventiva o reattiva? La risposta in generale che deve fornire un ingegnere è sempre: "dipende". Ogni problema ha una storia diversa da un altro.

E' bene ricordare che **non** esistono modelli precisi perchè nei sistemi distribuiti ci sono troppi parametri da prendere in considerazione: famiglia del processore, sistema operativo, linguaggio di programmazione etc

### Quali sono le sfide dell’enterprise computing?
- Portabilità
- Interoperabilità fra diversi ambienti (anche legacy)
- Supporto e gestione runtime: scalabilità, efficienza, tolleranza ai guasti, resilienza, affidabilità, qualità, etc
- Time-to-market
- Integrazione

### Come sono evolute le architetture per le applicazioni enterprise?
Le architetture si sono evolute sempre di più verso architettura N-tier perchè l'obiettivo è quello di separare logicamente le funzionalità in modo da ridurre la complessità degli strati:
- Single-Tier
- Two-Tier
- Three-Tier

### Cos’è J2EE?
E' un insieme di specifiche le cui implementazioni vengono principalmente sviluppate in linguaggio di programmazione Java e ampiamente utilizzata nella programmazione Web.

Esistono diversi software open source, che vengono spesso usati anche in ambiente di produzione:
- GlassFish: è l'implementazione di riferimento, mantenuta da Oracle.
- WildFly: precedentemente noto come JBoss
- Apache TomEE: del quale esiste una versione avente supporto commerciale chiamata Tomitribe

### Come funzionano i modelli a contenimento?

## 02-EJB_basics

### Che cos’è EJB? Che caratteristiche offre? Quali sono i principi di design di EJB? Che benefici offre EJB?

### Che problemi ha avuto EJB 2.X?

### Quale è la sua architettura? Quali contratti esistono?

### Che funzionalità offre l’EJB container? Di cosa si occupa?

### Quali sono le tipologie di componenti Bean?

### Cos’è un Session Bean? Quando va usato? Che tipi di Session Bean esistono?

### Cos’è un Entity Bean? Come si usano? Come può essere gestita la persistenza?

### Cosa sono e come funzionano i Message Driven Bean?

### Spiegare le interfacce EJBHome ed EJBObject

### Come agisce un cliente in EJB 2.X?

### Come avviene l’invocazione remota in EJB 2.X? Quali sono gli oggetti in gioco?

### Come funziona l’uso locale di EJB?

### Come avviene il deployment di una applicazione EJB?
