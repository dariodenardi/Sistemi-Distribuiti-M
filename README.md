# Sistemi Distribuiti M - 2021/2022

## 01-Modelli

### Che cosa è un componente?
E' un "pezzo" di software che viene scritto dallo sviluppatore ed ha le seguenti caratteristiche:
- Contiene stato, metodi etc ma espone verso l'esterno solo quei metodi che si decidono che siano visibili all'esterno grazie all'uso di un'interfaccia;
- Viene eseguito all'interno di un container/engine/middleware.

Esempi di componenti sono quelli che si usano per creare le interfacce grafiche con JavaFX: textBox, label, comboBox etc. Quando si clicca su un bottone non si verifica se effettivamente il mouse è sopra al tasto e lo si schiaccia ma si scrive solo il codice che deve essere eseguito se quel determinato evento si verifica.

- componente nel concentrato: un componente che fa parte di un'applicazione che viene messa in esecuzione su una sola macchina.
- componente nel distribuito: un concetto più ampio rispetto a quello del concentrato. Il componente non è vincolato a trovarsi su una sola macchina proprio per la definizione intrinseca di sistema distribuito. E' possibile spostarelo in qualsiasi momento da un nodo ad un altro.

### Che differenza c'è tra un componente e un oggetto?
Uguale all'oggetto:
mantiene dettagli di come è implementato: stato, metodi ed espone solo alcuni dei suoi dettagli (interfaccia)

Diverso dall'oggetto:
- il componente viene eseguito all'interno di un container
- Grana grossa: il componente è di dimensioni più grandi di un oggetto perchè il costo di overhead tra un'interazione e l'altro è maggiore. Ipotizziamo che due componenti A e B interagiscono tra di loro. A per comunicare con B deve instaurare una connessione, scambiare i dati e alla fine chiuderla. Nel concentrato, invece, gli oggetti anche se sono piccoli e interagiscono spesso fra di loro non c'è questo problema di overhead.

### A cosa siamo interessati nei sistemi distribuiti?
Siamo interessati alle performance e ad eventuali colli di bottiglia. Per poterli evitare è necessario osservare le performance dell'applicazione e solo in seguito si capisce che cosa andare a modificare.

Le modifiche non si effettuano all’interno del codice stesso ma attraverso l'operazione di deployment (dispiegamento). Ad esempio, installare librerie se servono librerie, copiare dei file che devono essere locali all’applicazione che lavora, distribuirlo su più nodi e mettere davanti un bilanciatore di carico etc.
Quando faccio deployment occorre decidere dove fare eseguire il pezzo di software e quali risorse dargli. Ad esempio, quando devo fare un Web App, ho un file che descrive queste scelte, qual è il mapping tra un URL e un determinato componente software e via dicendo. Come lo faccio? Ci sono diversi approcci:
- Manuale: l’utente determina ogni singolo oggetto/componente e lo trasferisce sui nodi appropriati
- File Script: si devono eseguire alcuni file di script racchiudono la sequenza dei comandi per arrivare alla configurazione che presenta le dipendenze
- Linguaggi dichiarativi: supporto automatico alla configurazione attraverso linguaggi dichiarativi o modelli di funzionamento della configurazione da ottenere

Una volta agganciato un pezzo di software ad una risorsa, questa decisione può essere:
- Statica: decisione presa dopo il deploy non cambia più.
- Dinamica: fatta solo quando la risorsa esterna serve
- Mista: partire con una decisione statica rivalutata a run-time -> Allocazione può essere varia.

Esistono anche approcci misti.
Nel distribuito diventa più complesso il deployment in quanto se sposto un pezzo di software da un nodo ad un altro devo poi stare attento a copiare tutte le dipendenze che gli servono. Nel distribuito spostare le cose ha degli impatti importanti.

### Che tipi di modelli possiamo utilizzare?
Ogni problema presenta una soluzione diversa. Per capire meglio come risolverlo è importante capire il funzionamento dei modelli.
I modelli che possono essere usati sono:
- statica o dinamica
- preventiva o reattiva

Tuttavia, è bene ricordare che non esistono modelli precisi perchè nei sistemi distribuiti ci sono troppi parametri da prendere in considerazione: famiglia del processore, sistema operativo, linguaggio di programmazione etc

### Quali sono le sfide dell’enterprise computing?
- Portabilità
- Interoperabilità fra diversi ambienti (anche legacy)
- Supporto e gestione runtime: scalabilità, efficienza, tolleranza ai guasti, resilienza, affidabilità, qualità, etc
- Time-to-market
- Integrazione

### Come sono evolute le architetture per le applicazioni enterprise?
Le architetture si sono evolute sempre di più verso architettura N-tier perchè l'obiettivo è quello di separare logicamente le funzionalità in modo da ridurre la complessità degli strati.
- Single-Tier
- Two-Tier
- Three-Tier

### Cos’è J2EE?

### Come funzionano i modelli a contenimento?
