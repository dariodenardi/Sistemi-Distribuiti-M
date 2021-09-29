# Sistemi Distribuiti M - 2021/2022

## 01-Modelli

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

## 02-EJB_basics

### Che cos’è EJB?

E' una tecnologia a componenti server-side che consente di creare applicazioni di tipo enterprise

E' una tecnologia ancora usata ma si inserisce in un contesto in cui ci sono moltre altre

### Che caratteristiche offre? Quali sono i principi di design di EJB? Che benefici offre EJB?


### Che problemi ha avuto EJB 2.X?


### Quale è la sua architettura?
- **Logica di business**: scritta dallo sviluppatore
- **Runtime**: avrò sul nodo che ospita EJB Server non solo le istanze che il programmatore ha scritto ma anche altri due oggetti che vengono automaticamente generati:
    - **Oggetto EJB Home**: implementa l’interfaccia EJB Home. In terminologia J2EE si dice che il cliente implementa la Home Interface. È un proxy che intercetta la chiamata del cliente (la prima volta) e decide quale istanza logica gli deve restituire (una già creata, nuova…);
    - **Oggetto EJB Object**: implementa l’interfaccia EJB Object. In terminologia J2EE si dice che il cliente implementa la Remote Interface. È un proxy che ha la stessa interfaccia del componente EJB creato dallo sviluppatore. Quando invoco un metodo chiamo un EJB Object che invoca poi il Java Bean;
    - **Macchina server**: non è pura perché mi serve un container per far girare la mia applicazione

Pensiamo di sviluppare una Web App riguardante la banca dove un utente può solo prelevare e depositare soldi:
- Sviluppatore: creo solo la classe che chiamiamo Account. Non mi interessa niente riguardo le istanze, allocazione/deallocazione e thread. Scrivo il codice come se avessi solo un cliente come ci viene detto dalla specifica. A tutto questo ci pensa tutto il container;
- In EJB 2.x ad ogni classe creata, viene automaticamente generata una classe EJB Home e EJB Object perché così dice la specifica (contratto cliente);
- Cliente: C1, C2 e C3 richiedono il prelievo. Da specifica per conoscere EJB Home è importante che sia disponibile nel sistema dei nomi;
- C1 fa richiesta di prelievo e invoca su EJB Home create/find. Arriva a EJB Home (estensione di rmi.remote) che crea un oggetto O1 ed è l’istanza logica/concettuale dedicata per C1. EJB Home restituisce al cliente il riferimento di EJB Object;
- L’invocazione del metodo prelievo verrà fatta su EJB Object che a sua volta potrà invocare l’oggetto O1;
- C3 fa una richiesta. EJB Home (non c’è scritto da specifica se EJB Home è uno solo o di più) potrà creare un nuovo oggetto O2 oppure dare il riferimento di O1 (non c’è scritto da specifica).

### Quali contratti esistono?
Esistono due tipi di contratto:
- **Client view contract**: contratto tra cliente e container. Un contratto client view è costituito da
    - Home interface:
    - Object interface:
    - Identità dell'oggetto? per arrivare alla home interface è necessario un servizio di nomi che mi consente di recuperare la home interface
- **Component contract**: contratto tra componente e container. Il contratto serve a gestire:
    - Abilita le invocazioni dei metodi dai clienti
    - Implementa le interfacce Home e Object per ridurre il carico di lavoro da parte dello sviluppatore
    - Gestisce la persistenza (solo in EJB 2.x da EJB 3.x la gestione cambia)
    - Gestisce tutti i servizi di sistema: sicurezza, transazionalità etc.
    - Implementa il meccanismo delle callback. Ci sono alcuni Bean che vengono attivati quando si riceve un determinato messaggio

E' trasparente passare da EJBHome a EJBLocalHome? No, perchè non c'è la RemoteException

### Che funzionalità offre l’EJB container? Di cosa si occupa?
Le funzionalità che offre sono molteplici:

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
E' un componente che consente di effettuare calcoli computazionali. 

### Cos’è un Entity Bean? Come si usano? Come può essere gestita la persistenza?


### Cosa sono e come funzionano i Message Driven Bean?

### Spiegare le interfacce EJBHome ed EJBObject


### Come agisce un cliente in EJB 2.X?


### Come avviene l’invocazione remota in EJB 2.X? Quali sono gli oggetti in gioco?

### Come funziona l’uso locale di EJB?

### Come avviene il deployment di una applicazione EJB?
