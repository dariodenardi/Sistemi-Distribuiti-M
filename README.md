# Sistemi Distribuiti M - 2021/2022

Corso tenuto dal _Prof. Foschini_

## 01.Modelli

###  (domanda provocatoria) Che cosa fa un laureato magistrale in ingegneria informatica in azienda oggi?

Comanda 😎

### Che cosa è un componente?

E' un _pezzo di software_ che viene scritto dallo sviluppatore ed ha le seguenti caratteristiche:
- Contiene stato, metodi etc. ma espone verso l'esterno solo quei metodi che si decidono che siano visibili all'esterno grazie all'uso di un'interfaccia;
- Viene eseguito all'interno di un _container_/_engine_/_middleware_.

Esempi di componenti sono quelli che si usano per creare le interfacce grafiche con **JavaFX**: _textBox_, _label_, _comboBox_ etc. Quando si clicca su un bottone non si verifica se effettivamente il _mouse_ è sopra al tasto e lo si schiaccia ma si scrive solo il codice che deve essere eseguito se quel determinato evento si verifica.

- **Componente nel concentrato**: un componente che fa parte di un'applicazione che viene messa in esecuzione su una **sola** macchina;
- **Componente nel distribuito**: un concetto più ampio rispetto a quello del concentrato. Il componente non è vincolato a trovarsi su una sola macchina proprio per la definizione intrinseca di sistema distribuito. E' possibile spostarlo in qualsiasi momento da un nodo ad un altro. Per questo motivo il componente nel concentrato viene visto come se appartenesse ad un'applicazione "monolitica".

### Che differenza c'è tra un componente ed un oggetto?
**Uguale all'oggetto**:

- Mantiene dettagli di come è implementato: stato, metodi etc ed espone solo alcuni dei suoi dettagli tramite l'interfaccia;

**Diverso dall'oggetto**:
- Il componente viene eseguito all'interno di un _container_/_engine_/_middleware_ altrimenti non verrebbe eseguita la _funzione di callback_. Se si eseguisse il codice di un componente su una qualsiasi JVM non funzionerebbe. Riprendendo l'esempio di prima: chi è che controlla che effettivamente il mouse è posizionato sopra al bottone? Nessuno quindi il codice non potrebbe funzionare;
- Il componente è di dimensioni più grande di un oggetto  in termini di codice, perchè il costo di overhead tra un'interazione e l'altra è maggiore. Ipotizziamo che due componenti A e B interagiscano tra di loro. A per comunicare con B deve instaurare una connessione, scambiare i dati e alla fine chiuderla. Nel concentrato, invece, gli oggetti anche se sono piccoli e interagiscono spesso fra di loro non hanno questo problema di overhead.

Ovviamente bisogna stare attenti a creare un componente non troppo grande per evitare di andare incontro a tutti quei problemi affrontati durante il corso di Ingegneria del Software T (riusabilità etc).

### Che tipi di modelli possiamo utilizzare?

Ogni problema presenta una soluzione diversa. Per capire meglio come risolverli è importante capire il funzionamento dei modelli.
I modelli che possono essere usati sono ad esempio:
- **Statici/dinamici**: sicuramente un sistema dinamico consente di adeguarsi a fronte di variazioni mentre un sistema statico no;
- **Preventivi/reattivi**: un sistema preventivo è più costoso di un sistema reattivo perchè non è detto che un evento si verifichi. Ad esempio, i sistemi operativi non utilizzano sistemi preventivi. Se avviene un deadlock tra processi lo si sblocca dall’esterno tramite linea di comando.

E' meglio una soluzione statica o dinamica? Meglio una soluzione preventiva o reattiva? La risposta in generale che deve fornire un ingegnere è sempre: _dipende_. Ogni problema ha una storia diversa. Questo perchè **non** esistono formule precise nei sistemi distribuiti dato che ci sono troppi parametri da prendere in considerazione: famiglia del processore, sistema operativo, linguaggio di programmazione etc.

### A cosa siamo interessati nei sistemi distribuiti?

Siamo interessati alle performance e ad eventuali colli di bottiglia. Per poterli evitare è necessario osservare le performance dell'applicazione e solo in seguito si capisce che cosa andare a modificare. Ad esempio, aprire troppe connessioni con un DB può introdurre un potenziale _bottleneck_.

Le modifiche non si effettuano sul codice stesso ma attraverso l'operazione di _deployment_ (dispiegamento). Ad esempio, installare tutte le librerie necessarie che servono all'applicazione, copiare i file che devono essere locali all’applicazione, distribuire i componenti su uno o più nodi e mettere davanti un bilanciatore di carico etc.
Quando faccio _deployment_ occorre decidere dove fare eseguire il componente e quali risorse ha bisogno per funzionare correttamente. Ad esempio, quando devo fare una Web App, ho un file che descrive queste scelte.

Come lo faccio? Ci sono diversi approcci:
- **Manuale**: l’utente determina ogni singolo oggetto/componente su quale è il nodo più appropriato;
- **File Script**: si devono eseguire alcuni file di script che racchiudono la sequenza dei comandi per arrivare alla configurazione che presenta le dipendenze;
- **Linguaggi dichiarativi**: supporto automatico alla configurazione attraverso linguaggi dichiarativi o modelli di funzionamento della configurazione da ottenere. Ad esempio, tramite il _file di deployment_ e annotazioni.

### Quali sono le sfide dell’enterprise computing?

- Portabilità;
- Interoperabilità fra diversi ambienti (anche _legacy_);
- Supporto e gestione runtime: scalabilità, efficienza, tolleranza ai guasti, resilienza, affidabilità, qualità, etc;
- Time-to-market: sviluppare componenti nel minor tempo possibile;
- Integrazione.

### Come sono evolute le architetture per le applicazioni enterprise?

Le architetture si sono evolute sempre di più verso architettura N-tier perchè l'obiettivo è quello di separare logicamente le funzionalità in modo da ridurre la complessità degli strati:
- **Single-Tier**: c'è un singolo calcolatore a cui sono connessi i clienti
    - **Vantaggi**: nessun aggiornamento dei clienti, una sola copia dei dati che si trova nel server;
    - **Svantaggi**: no scalabilità.
- **Two-Tier**:
- **Three-Tier**:

### Cos’è J2EE?

E' un insieme di specifiche le cui implementazioni vengono principalmente sviluppate in linguaggio di programmazione Java e ampiamente utilizzata nella programmazione Web. Viene scritta solo la specifica non l’implementazione, diversi produttori di software hanno fatto diverse implementazioni; purché siano JEE compliant basta che rispettino la specifica. Alcune implementazioni si limitano alla specifica altre aggiungono funzionalità.

Esistono diversi _software open source_, che vengono spesso usati anche in ambiente di produzione come:
- **GlassFish**: è l'implementazione di riferimento mantenuta da Oracle;
- **WildFly**: precedentemente noto come JBoss.

### Come funzionano i modelli a contenimento?

Le chiamate clienti verso i metodi EJB sono intercettate dal container prima che questo le “deleghi” ai componenti EJB veri e propri.

---

## 02.EJB 2.x

E' una tecnologia a componenti lato _server-side_ che consente di creare applicazioni distribuite che siano multi-tier, 
transazionali, portabili, scalabili, sicure, etc.

EJB è stata una delle prime tecnologie a componenti e al giorno d'oggi, sebbene c'è ne siano molte altre, non è da considerarsi completamente in disuso.

### Principi di design di EJB

- Le applicazioni EJB e i loro componenti devono essere debolmente accoppiati (_loosely coupled_). Ad esempio, se si hanno due componenti A e B, A deve chiamare un metodo di B non molte volte. Questo perchè nel distribuito si paga un costo di _overhead_ piuttosto alto. Dall'altra parte, i componenti devono essere portabili, quindi bisogna stare attenti a come si scrive il software altrimenti diventa difficile ricostruire le sue dipendenze quando si decide di usare quel codice in un altro progetto;
- Il comportamento dei componenti EJB è definito tramite interfacce. Concetto assolutamente non nuovo perchè basti pensare alla normale programmazione ad oggetti;
- Lo sviluppatore **non** deve pensare a come gestire le risorse. Ci pensa tutto il container;
- Le applicazioni EJB sono N-tier.

### Quale è la sua architettura?

L'immagine di seguito riportata, fa riferimento ad un'architettura three-tier: c'è un cliente, un server e un database.

![ejb2_architettura](./img/ebj2_architettura.png)

È bene ricordare che la macchina server non è "pura" perché è necessario installarci sopra anche un container per far girare l'applicazione costituita da componenti.

Sul'EJB Container non si troveranno solo le istanze che il programmatore ha scritto ma anche altri due oggetti che vengono automaticamente generati:

- **Oggetto EJB Home**: implementa l’interfaccia EJBHome. In terminologia J2EE si dice che il cliente implementa la Home Interface. È un _proxy_ che intercetta la chiamata del cliente (la prima volta) e decide quale istanza logica gli deve restituire (una già creata, nuova etc.);
- **Oggetto EJB Object**: implementa l’interfaccia EJBObject. In terminologia J2EE si dice che il cliente implementa la Remote Interface. È un _proxy_ che ha la stessa interfaccia del componente EJB creato dallo sviluppatore. Quando si invoca un metodo, si chiama l'EJBObject che invoca poi a sua volta il metodo del componente scritto dal programmatore.

Ad esempio, si consideri un'applicazione riguardante una banca dove un utente può solo prelevare e depositare soldi:

- **Sviluppatore**: crea solo una classe che chiama _Account_. Al suo interno ci sono i metodi _preleva_ e _deposita_. Non bisogna occuparsi dell'allocazione/deallocazione delle istanze, della concorrenza etc ma si scrive il codice come se si avesse solo un cliente. A tutto il resto ci pensa il container. In EJB 2.x ad ogni classe creata, bisogna anche creare due interfacce: EJBHome e EJBObject;
- **Cliente**: si ipotizzi di avere tre clienti: C1, C2 e C3 che richiedono tutti di eseguire il metodo _preleva_. Per conoscere EJBHome è importante che sia disponibile nel sistema dei nomi:
    - C1 fa richiesta di prelievo, dovrà per prima cosa tramite il servizio di nomi ottenere EJBHome (in realtà si ottiene l'oggetto _stub_). Dopo ,invoca su EJBHome _create()_/_find()_. La richiesta arriva a EJBHome che crea un oggetto O1 ed è l’istanza logica dedicata per C1. EJBHome restituisce al cliente il riferimento di EJB Object (riferimento allo _stub_);
    - L’invocazione del metodo prelievo verrà fatta su EJBObject che a sua volta potrà invocare l’oggetto O1;
    - C3 fa una richiesta. EJBHome potrà creare un nuovo oggetto O2 oppure dare il riferimento di O1. Non è detto che debba essere lo stesso.

### Contratti in EJB 2.x

Esistono due tipi di contratto:

- **Client view contract**: contratto tra cliente e container. Un contratto _client view_ è costituito da:
    - **Home interface**: _proxy_ che funge da vera e propria factory;
    - **Object interface**: _proxy_ che ha gli stessi metodi di business della classe sviluppata dal programmatore;
    - **Identità dell'oggetto**? per arrivare alla home interface è necessario un servizio di nomi che consente di recuperare la home interface.
- **Component contract**: contratto tra componente e container. Il contratto serve a:
    - Abilitare le invocazioni dei metodi dei clienti;
    - Implementare le interfacce EJBHome e EJBObject per ridurre il carico di lavoro da parte dello sviluppatore;
    - Gestisce la persistenza (solo in EJB 2.x, da EJB 3.x la gestione è diversa);
    - Gestisce tutti i servizi di sistema: sicurezza, transazionalità etc.;
    - Implementa il meccanismo delle callback. Ci sono i Message Driven Bean che vengono attivati quando si riceve un determinato messaggio.

### EJB container

Come è stato detto in precedenza, lo sviluppatore si occupa solo di sviluppare 

- Genera automaticamente le classi concrete delle interfaccia EJBHome e EJBObject;
- Effettua il _binding_ dell’oggetto Home presso il
servizio di naming;
- Persistenza;
- Transazionalità;
- Gestione _lifecycle_ componenti;
- _Connection pooling_;
- _Threading_;
- Sicurezza.

### Tipologie di componenti Bean

I Bean possono essere classificati in due categorie:

- **Sincroni**: l'utente si blocca e aspetta la risposta da parte del _server_. Esistono due tipi di componenti sincroni:
    - **Session Bean**: si possono ulteriormente classificare in:
        - **Stateful**;
        - **Stateless**.
    - **Entity Bean**: a sua volta esistono due tipi di Entity Bean:
        - **Container Managed Persistence (CMP)**;
        - **Bean Managed Persistence (BMP)**.
- **Asincroni**: l'utente non si blocca e non aspetta la risposta. Esiste un solo tipo di componente che fa parte di questa categoria:
    - **Message Driven Bean**.

### Session Bean

Un Session Bean ha le seguenti caratteristiche:

- Rappresenta un processo di business;
- Ogni cliente ha un'istanza logica dedicata;
- _Short-lived_: la vita del bean è pari alla vita cliente o al massimo alla durata della sessione;
- Transient;
- No _fault-tollerant_: non sopravvive a crash del server;
- Può avere proprietà transazionali;
- Implementa l’interfaccia _javax.ejb.SessionBean_.

Questo componente viene usato quando bisogna effettuare calcoli computazionali

I Session Bean che esistono sono di due tipi:

- **Stateless**: il componente è privo di stato. Ad esempio, quando un'azione deve essere idempotente;
- **Statefull**: il componente è con stato. Ad esempio, quando si aggiungono i prodotti in un carrello di un sito e-commerce.

### Entity Bean

Un Entity Bean ha le seguenti caratteristiche:
- Rappresenta i dati di business;
- L'stanza è condivisa fra clienti diversi;
- Long-lived: la vita del bean è pari a quella dei dati nel database;
- Persistente;
- Fault-tollerant: sopravvive a crash del server;
- Sempre transazionale;
- implementa l’interfaccia _javax.ejb.EntityBean_.

Gli Entity Bean che esistono sono di due tipi:
- **Container Managed Persistence (CMP)**: persistenza gestita completamente dal container. Requisiti di persistenza specificati interamente nel descrittore di deployment;
- **Bean Managed Persistence (BMP)**: è responsabilità dello sviluppatore la gestione della persistenza.

### Message Driven Bean

- Svolgono il ruolo di consumatori di messaggi asincroni;
- Non possono essere invocati direttamente dai clienti;
- Attivati in seguito all’arrivo di un messaggio;
- Non hanno interfacce EJBHome e EJBObject;
- I clienti possono interagire con i Message Driven Bean tramite l’invio di messaggi verso le code o i topic per i quali questi componenti sono in ascolto (listener);
- Privi di stato.

### Interfacce EJBHome ed EJBObject

- **Interfaccia EJBHome**: è un _proxy_ che intercetta la chiamata del cliente (la prima volta) e decide quale istanza logica gli deve restituire (una già creata, nuova etc.). Al suo interno avremo i metodi per la creazione, il ritrovamento e la distruzione del bean. Tuttavia, il programmatore definisce solo l'interfaccia mentre l'oggetto è implementato dal container. L'interfaccia può essere remota e/o locale (vedi domande successive del perchè).
Il cliente ottiene il riferimento all’oggetto _stub_
dell’oggetto EJBHome tramite JNDI;

- **Interfaccia EJBObject**: È un _proxy_ che ha la stessa interfaccia del componente EJB creato dallo sviluppatore. Quando invoco un metodo chiamo l'EJBObject che invoca poi a sua volta il Java Bean. Il programmatore definisce solo l'interfaccia mentre l'oggetto è implementato dal container. L'interfaccia può essere remota o locale (vedi domande successive del perchè).
Il cliente ottiene il riferimento all’oggetto _stub_ di EJBObject attraverso i metodi create() o find() dell’interfaccia EJB Home.

### Cliente in EJB 2.X

Per interagire con un componente EJB il cliente deve:
- Ottenere l’oggetto EJBHome (in realtà un oggetto _stub_ per l’oggetto EJBHome) via JNDI perchè la comunicazione tra client e server avviene tramite RMI;
    - Creare l'oggetto InitialContext. Questo oggetto serve per poter cercare sul servizio di nomi;
    - Effettuare la lookup sul servizio di nomi;
    - Effettuare il narrowing;
- Dall’oggetto EJBHome, si invoca la _create()_ in modo da ottenere l'istanza logica dedicata dell'oggetto EJB desiderato. In realtà, si ottiene un oggetto _stub_ per l’oggetto EJBObject per lo stesso motivo di prima;
- Invocare i metodi di business tramite l’oggetto EJB;
- Effettuare il clean up finale per liberare le risorse. Perchè occupare un'istanza che non si usa?

### Invocazione remota in EJB 2.X

Per prima cosa bisogna implementare le interfacce EJBHome e EJBObject. Ovviamente gli oggetti che cooperano, in questo caso, si trovano su JVM differenti. Dal lato cliente vengono invocati i metodi di oggetti lato server e necessariamente ci deve essere un meccanismo di comunicazione tra cliente e server.

RMI è utilizzato per la comunicazione fra cliente e server EJB. Le operazioni RMI sono costose perchè bisogna effettuare la serializzazione/deserializzazione dei parametri, aprire, trasferire e chiudere una connessione RMI bastato su IIOP

In più nello specifico dato che la comunicazione avviene con RMI su IIOP i passaggi sono i seguenti:

- Cliente:
    - Invoca un metodo dell’oggetto remoto
    - Lo _stub_ dell’oggetto remoto
        - “Intercetta” l’invocazione di metodo
        - Effettua il _marshalling_ dei parametri
        - Effettua la chiamata vera e propria all’oggetto remoto
- Oggetto remoto:
    - Riceve l’invocazione tramite il suo _skeleton_
    - Effettua l’_unmarshalling_ dei parametri
    - Esegue l’invocazione localmente
    - Effettua il _marshalling_ dei risultati e li invia al cliente
- Lo _stub_ dell’oggetto remoto:
    - Riceve i risultati,effettua un marshalling e li restituisce al cliente

IIOP è un protocollo di comunicazione del mondo CORBA.
c'è una visione in RMI del mondo CORBA. Tuttavia, CORBA ha un suo standard.

### Invocazione locale in EJB

Per prima cosa bisogna ricordarsi di implementare le interfacce EJBLocalHome e EJBLocalObject. Ovviamente, in questo caso i metodi non producono *RemoteException*.
Le interfacce locali, si usano quando il cliente esegue nella stessa JVM del componente EJB di interesse (e del suo container). Ad esempio, quando lo sviluppatore deve scrivere il codice. Non avrebbe senso pagare i costi di overhead sulla stessa macchina. In questo caso il passaggio dei parametri può avvenire tramite riferimento proprio perchè ci troviamo sulla stessa macchina

Inoltre, c'è un'altro possibile uso per rendere più efficiente il funzionamento di un Session Bean. Esso può svolgere a sua volta il ruolo di "cliente local" verso altri bean.

Questa possibilità è stata introdotta a partire da EJB2.0 anche se alcune implementazioni avevano già delle ottimizzazioni senza che fosse inserito ufficialmente nello standard. Anche guardando lo _skeleton_ di RMI ha un'idea di ottimizzazione. Se è locale evito di comunicare remotamente.

E' bene ricordare anche che non è trasparente passare da EJBHome a EJBLocalHome perchè l'interfaccia locale non ha la RemoteException

### Deployment di una applicazione EJB

Per effettuare il deployment di un'applicazione EJB sono necessari i seguenti file:

- ***.EAR (Enterprise ARchive)**: è tutta l'applicazione EJB che si trova lato server. Al suo interno ci sono i seguenti file:
    - ***.WAR (Web ARchive)**: modulo Web (Servlet, JSP etc. ). E' facoltativo perchè non è detto che lo si debba inserire;
    - **EJB-JAR (*.jar)**: modulo EJB al cui interno è possibile inserire uno o più componenti. Dipende come si vuole organizzare il progetto. Ad esempio, se all'interno si inserisce un solo componente, questo modulo lo si potrà trasferire. Il file ejb-jar deve contenere almeno i seguenti file:

        - **Classe bean**: classe scritta dallo sviluppatore;
        - **Interfaccia EJBHome**: nel modulo verrà inserita solo l'interfaccia. A tempo di esecuzione poi è necessario che il container implementi lo _stub_
        - **Interfaccia EJBObject**: a tempo di compilazione servono le interfacce mentre a tempo di esecuzione poi è necessario che il container implementi lo _stub_
        - **application.xml**: descrittore di deployment. La visibilità è "locale" perchè si limita all'interno del modulo.

        Per effettuare il deployment di una applicazione EJB, è sempre necessario creare un file *.EAR anche se l’applicazione prevede un solo file EJB-JAR e nessun modulo Web. Tuttavia, alcuni container permettono il deployment diretto del solo modulo EJB-JAR senza dover creare il file *.EAR.

    - **application.xml**: descrittore di deployment. La visibilità è "globale" paragonato a quello che si trova all'interno di ogni modulo.
- ***.JAR**: cliente EJB

### Problemi riscontrati in EJB 2.X

La comunità di sviluppatori ha riscrontrato una serie di problemi che sono emersi durante l'uso di questa tecnologia:

- Programmazione a componenti basata su modello POJO (Plain Old Java Objects), più vicina quindi al modello tradizionale di programmazione
- Il modello di programmazione non sempre naturale: oltre alla logica di business bisogna anche implementare due interfacce. L'obiettivo è quello di scrivere in modo molto più simile un componente a come si fa con gli oggetti. Inoltre, bisogna ricordare di configurare il file descriptor che è un file diverso a quello in cui si scrive il codice della classe;
- La lookup dei componenti è sempre basata su JNDI
- Difficoltà di uso corretto (vedi qualche antipattern emerso, soprattutto per gli entity bean) (finire di scrivere questo punto)

Tuttavia, prima di passare a spiegare EJB 3.X, bisogna introdurre prima alcuni concetti. Nei prossimi due capitoli, si parlerà di annotazioni e di sistema di nomi.

---

## 03.Annotazioni

Le annotazioni sono state già viste sicuramente in altri corsi anche se non si sapeva che si chiamassero in questo modo. Di seguito vengono riportati alcuni esempi:
- **@Overrided**
    ```
    @Override
    public String toString() {
        ...
    }
    ```
- **@Deprecated**
    ```
    @Deprecated
    public class ExampleClass { ... }
    ```
- **@SuppressWarnings**
    ```
    @SuppressWarnings(“unchecked”)
    public void aMethod() { ... }
    ```

### Definizione

Sono metadati con cui si decorano i metodi, le classi, le interfacce etc. Non modificano il comportamento del codice che lo sviluppatore scrive ma aggiungono solo informazioni che possono essere utili:
- Al compilatore;
- Alla _Javadoc_;
- A _runtime_.

### Esempi

Riprendendo le annotazioni scritte all'inizio del capitolo:

- **@Overrided**: serve al compilatore. Se non si mettesse questa annotazione, il codice verrebbe generato lo stesso però bisogna stare attenti a come si scrive il nome del metodo sia nella classe padre che in quella figlia perchè se si sbagliasse a scrivere il nome, il metodo non verrebbe sostituito ma aggiunto;
- **@Deprecated**: serve a livello di documentazione per indicare che quel metodo, classe etc. è in disuso;
- **@SuppressWarnings**: serve al compilatore. Se la compilazione presenta dei _warning_ questi vengono trascurati e non mostrati all’utente.

### Sintassi

Le annotazioni sono strutturate nel seguente modo: ci può essere solo il nome dell'annotazione o in caso ci fossero dei membri vengono scritti come un insieme di coppie _nome=valore_. Se il membro è solo uno, il nome si può omettere.

Con **membro** si intende il _parametro di ingresso_ dell'annotazione.

Nel paragrafo della definizione, è stata usata la parola _decora_ proprio perchè vengono aggiunte all'esterno del metodo, classe, interfaccia. Ad esempio:

```
@Override
public String toString() {
    ...
}
```

### Categorie di annotazioni

Le annotazioni si possono classificare nel seguente modo:

- **Marker annotation**: non hanno membri. Ad esempio: @Override;
- **Single-value annotation**: hanno un solo membro. Ad esempio: @SuppressWarnings("unchecked");
- **Full annotation**: l'annotazione è formata da più di un membro;
- **Custom annotation**: i programmatori possono crearsi le proprie annotazioni.

### Annotazioni personalizzate

Di seguito viene riportato un esempio di come scrivere un'annotazione personalizzata:

```
public @interface GroupTODO {
    public enum Severity {CRITICAL, IMPORTANT, TRIVIAL} ;
    Severity severity() default Severity.IMPORTANT;
    String item();
    String assignedTo();
}
```

Si noti che le annotazioni personalizzate sono di tipo @interface.

Adesso, la si può aggiungere ad esempio ad un metodo:

```
@GroupTODO (
    severity = GroupTODO.Severity.CRITICAL;
    item = "Figure out the amount of interest per month"
    assignedTo = "Luca Foschini";
)
public void calculateInterest(float amount, float rate) { ... }
```

Questa annotazione fa parte della categoria delle annotazioni personalizzate e presenta tre membri. Se non fosse stato specificato il membro _severity_ il suo valore sarebbe stato IMPORTANT.

#### Limiti delle annotazioni personalizzate

- **Non** si possono avere relazioni di estensione (_extends_) fra tipi di annotazioni;
- I tipi di ritorno degli eventuali metodi di una annotazioni devono essere: tipi primitivi, String, Class, enum, tipi di annotation o array dei tipi appena elencati;
- Una annotation **non** può lanciare eccezioni ovvero non può avere una _throws clause_;
- **Non** sono permessi _self-reference_. Ad esempio: AnnotationA non può contenere un membro di tipo AnnotationA;
- **Non** sono permessi _circular-reference_. Ad esempio: AnnotationA non può contenere un membro di tipo AnnotationB e quest'ultimo di AnnotationA.

### Meta-annotazioni

Sono annotazioni che si specificano sulle annotazioni che vengono create. Le meta-annotazioni sono:

- **@Target**: specifica il tipo di elemento al quale si può allegare tale tipo di annotazione (campo, metodo, classe, interfaccia etc.);

    ```
    @Target ( { ElementType.METHOD,ElementType.PACKAGE } ) public @interface ExampleAnnotation { ... }
    ```

- **@Documented**: specifica che le annotazioni di tale tipo faranno parte della Javadoc;

    ```
    @Documented
    public @interface ExampleAnnotation { ... }
    ```

- **@Inherited**: questo tipo di annotazione funziona **solo** se apposta ad una classe. Il tipo di annotazione verrà automaticamente ereditato dalle sottoclassi della classe alla quale viene allegata;

    ```
    @Target ( { ElementType.METHOD,ElementType.PACKAGE } ) public @interface ExampleAnnotation { ... }
    ```

- **@Retention**: politica di mantenimento in memoria con cui il compilatore e JVM devono gestire le annotazioni.

    ```
    @Inherited
    public @interface ExampleAnnotation { ... }
    ```

#### Politiche di retention

- **@Retention(RetentionPolicy.SOURCE)**: l'annotazione permane solo a livello di codice sorgente. Dunque, non viene memorizzata nel bytecode cioè nel file .class. Viene utilizzata solo a tempo di sviluppo da parte del compilatore. Ad esempio, l'annotazione @Override. Se confrontassi la dimensione del file in cui l'annotazione è stata scritta e quello in cui l'annotazione non è stata scritta, potremmo vedere che è la stessa;
- **@Retention(RetentionPolicy.CLASS)(default)**: l'annotazione verrà registrata nel bytecode ma non verrà mantenuta dalla JVM a runtime. Dunque, non si può usare la reflection ma solo a tempo di caricamento. Ad esempio, si può decidere come trattare il caricamento tramite il class loader del bytecode ma poi le annotazioni non si possono sono usate a run-time
- **@Retention(RetentionPolicy.RUNTIME)**: l'annotazione verrà registrata nel bytecode e potrà essere letta a runtime tramite reflection anche dopo il caricamento della classe da parte della JVM. E' utilizzabile anche all’interno del codice di supporto/applicativo a tempo di esecuzione, con proprietà eventualmente modificabili a runtime.

### Perchè usarle

Le annotazioni arricchiscono lo spazio concettuale di un linguaggio di programmazione. Consentono di fare programmazione dichiarativa oltre che a quella imperativa perchè permettono di associare delle informazioni in modo dichiarativo al codice non andando a modificare il comportamento dei metodi, delle classi etc. Ad esempio, quello che viene specificato nel di *file di deployment* si può fare benissimo tramite le annotazioni.

In poche parole, si aggiunge una "parola" al codice della classe senza combiare la logica di business.

## 04.Sistemi di Nomi

### Cos’è un servizio di naming?

Un servizio di naming è un sistema che consente di associare ad un nome logico una risorsa (nome fisico, riferimento, oggetto).

Esempi di sistemi di nomi:
- DNS;
- RMI Registry (RMI);
- Portmapper (RPC)

Nome logico (numero di programma), restituisce il numero di porta

### Cos’è un protocollo di discovery? Fornire un esempio

Un protocollo di discovery è un protocollo in cui si invia una richiesta di broadcast

Esempio: il Bluetooth usa un protocollo di discovery.

### Cosa sono le directory? Come sono fatte? Fornire un esempio

Directory: LDAP, es. accesso ai laboratori di UNIBO

X500

Perchè directory e non db: 15.33
limitare il numero in spazio e in tempo della ricerca

31
una risorsa che non può essere serializabile: database, file, stampante è necessario avere il concetto di riferimento remoto

### Cos’è e cosa fa JNDI?

JNDI è un'interfaccia standard che consente di accedere in modo uniforme a servizi di naming già esistenti. Dunque, non è un servizio di nomi ma un'interfaccia!
In questo modo possiamo cambiare servizio di nomi senza preoccuparci del codice che viene scritto lato client. Basta solo modificare la parte di setting.

17
In RMI non è possibile avere due entry con lo stesso nome logico ma ci potrebbero essere dei sistemi di nomi che lo consentono

### Quali sono i concetti fondamentali in JNDI?

### Cos’è un provider JNDI?

### Cos’è l’interfaccia Context e che metodi ha? Esempi di sotto-interfacce di Context?

Context è interfaccia specifica che contiene metodi per aggiungere, cancellare, cercare, ridenominare oggetti.
InitialContext è un’implementazione di Context e rappresenta contesto di partenza per operazioni di naming

Alcuni servizi di nomi supportano subcontext (context dentro un altro context, come cartella in direttorio)

DirContext, sottoclasse di Context, estende le funzionalità standard di naming con altre relative a attributi e ricerche su entry di directory

### Esempio di uso di JNDI?

1. Occorre scegliere un naming service provider (ad es. OpenLDAP o un’altra implementazione di LDAP). Bisogna aggiungere il nome del provider all’insieme di proprietà di ambiente (in un oggetto Hashtable):

2.Aggiungere ogni info addizionale necessaria al naming provider (ad es. per LDAP, URL che identifica il servizio, context radice, nome e password per connessione):

3. Ottenere initial context. Se si intendono eseguire solo operazioni di naming, oggetto Context; altrimenti oggetto DirContext. Non tutti naming provider li supportano entrambi

### Come si possono memorizzare i dati in JNDI?

Il punto è che la specifica JNDI lascia a JNDI service provider la definizione della semantica dell’operazione di
memorizzazione di un oggetto. “Encouraged (but not required) to support object storage in one of the following formats:”
❑ Dati serializzati
❑ Riferimento
❑ Attributi in un directory context

### Configurazione di JNDI?

Per accedere a uno specifico naming/directory service, occorre specificare quale service provider utilizzare, quale server, ...
❑ Standard
❑ Service-specific
❑ Feature-specific 
❑ Provider-specific

Nel caso di proprietà presenti in più sorgenti, generalm. valori delle proprietà sono concatenati in una lista separata da virgole; per alcune proprietà viene preso solo primo valore assegnato

ora 17.00: Cosa si differenzia l'oggetto?

```
void bind(String stringName, Object object)
```

## 05.EJB3

Perchè sono importanti componenti asincroni?
Svolgono operazioni molto onerose in modo asincrono. La VM è asincrona perchè ci vuole tempo.

Perchè non ci piace l'intercettore?
Cambiare stati, spargere il codice su più metodi e diventa difficile controllare il codice. perchè mettere il codice di un bean dentro all'intercettore?

## 06.JPA

### Cos’è JPA e cosa fa? Perché JPA e non DAO?

JPA è la specifica Java standard che consente il supporto al mapping O/R.

Astrarre: usiamo le annotazioni semplificando il modello della programmazione
Vista performance: nel 2.1 la connessione era da gestire esplicitamente dallo sviluppatore. Nella 3 la gestione è demandata al container in modo da ottimizzare le risorse.

Ho un unico framework che può essere usato nel mondo Java (standard ed enterprise edition)

DAO: bisogna aprire la connessione, inviare la query, ottenere i risultati. Programmazione molto meno intuitiva.
Svantaggi:
- No POJO
- Ogni oggetto apre una connessione
- Tutto dipende dal Data Source

### Cos’è un’entità in JPA? Che caratteristiche ha? Come può essere la chiave primaria?

- annatazione javafx.persistence.Entity
- cotruttore senza argomenti public o protected
- nessun metodo deve essere final perchè i valori devono essere modificati

### Come funziona l’ereditarietà per le entity? Quali sono le strategie di mapping possibili per gli ORM? Che performance comportano?

MappedSuperclass: integrare con codice non scritto da noi


### Come vengono gestite le molteplicità nelle relazioni? E la direzionalità?

- **SINGLE_TABLE**: tutte i campi in un'unica tabella con un attributo chiamato discriminator che consente di stabilire il tipo di Entity. Questo può servire per risalire alla tipologia dell'oggetto che ci interessa. Scarsa efficienza se abbiamo molti NULL nella tabella
- **TABLE_PER_CLASS**: ogni tabella ha colonne per ogni proprietà comprese quelle ereditatte dalle superclassi. Non c'è bisogno del discriminator e non è uno schema normalizzato
- **JOINED**: ogni tabella ha le colonne con valore con le sole proprietà definite nella classe specifica ma lo schema è normalizzato (schema non ridondante). Se bisogna normalizzare i dati non c'è ridondanza ma dobbiamo effettuare le join. Se la gerarchia è estesa il costo diventa molto alto.

### Cos’è e di che tipi può essere l’EntityManager?



### Qual è il ciclo di vita di una entity? Approfondire persist, remove, ecc



### Come si possono creare le query?



### Cos’è una unità di persistenza e come si differenzia dal contesto di persistenza?



### Come può essere gestito il loading?



### Esempi delle operazioni + esempi di listener



### Cos’è Hibernate? Quali sono i principali oggetti di questo framework? Quale può essere lo stato degli oggetti persistenti?



### Come funziona il caching in Hibernate? Che tipo di cache esistono?



### Come funziona il concurrency control in Hibernate? Ed il version checking?