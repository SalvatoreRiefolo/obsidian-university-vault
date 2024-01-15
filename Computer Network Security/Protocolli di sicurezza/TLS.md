---
tags: 
  - computer-network-security
---

Evoluzione di Secure Sockets Layers (SSL), **Transport Layer Security (TLS)** è un servizio general purpose implementato come una serie di protocolli basati su TCP.
Questi protocolli possono essere divisi in due **layers**:
1. **Record Protocol**, che implementa le trasformazioni di sicurezza.
2. **Handshake, Alert, Change Cipher Spec**, per autenticare e configurare la connessione.

Due concetti importanti sono:
- **Sessione**: un'associazione tra client e server creata con il protocollo handshake. Definisce un insieme di parametrici crittografici condivisi a connessioni multiple, usati per evitare la negoziazione di nuovi parametri per ogni connessione.
- **Connessione**: sono relazioni peer-to-peer transienti, associate con una sessione.

Tra due parti possono esserci molteplici connessioni attive, solitamente associate ad una sessione.

### Sessione
Lo stato di una sessione è definito dai seguenti parametri:
- **Identificativo** della sessione.
- **Certificato X.509**.
- **Metodo di compressione** da applicare prima della cifratura.
- **Algoritmo di cifratura e di hash** da usare per cifrare e produrre MAC, con associati parametri.
- **Master secret** condiviso tra client e server.
- **IsResumable**, parametro che specifica se la sessione può essere usata per creare nuove connessioni.

### Connessione
Lo stato di una connessione è definito dai seguenti parametri:
- **Server/client random**, una sequenza di byte scelta per ogni connessione.
- **Server/client write MAC secret**, chiave usata per generare MAC da server/client.
- **Server/client write key**, chiave usata da server/client per cifrare e dal client/server per decifrare.
- **Vettori di inizializzazione**, inizializzati dal protocollo handshake di TLS ed usati da diversi algoritmi di cifratura/hash.
- **Numeri di sequenza** per messaggi trasmessi.

## TLS Record Protocol

È utilizzato direttamente da protocolli applicativi quali HTTP e IMAP, e può essere visto come un protocollo di presentazione (layer 6 dello stack ISO/OSI).

Implementa due trasformazioni di sicurezza per i dati applicativi:
- Confidenzialità, usando la chiave condivisa tramite handshake per cifrare i payload.
- Integrità, usando un'altra chiave per generare MAC, sempre condivisa tramite handshake.

Un messaggio da livello di applicazione viene frammentato, ed ogni frammento diventa un messaggio da inviare usando TLS: viene prima compresso, si calcola il MAC e la coppia viene cifrata. Il messaggio associato ad un header, chiamato Record Header, viene inviato.

## Altri protocolli

Alcuni protocolli sono definiti usando TLS Record Protocol. Sono protocolli da implementare al livello di sessione (layer 5).

### Change Cipher Spec Protocol
Consiste in un singolo messaggio di 1 byte di valore $1$. Lo scopo del messaggio è aggiornare il cifrario da usare per la connessione.

### Alert Protocol
Consiste in un messaggio di 2 bytes: il primo ha valore warning o fatal, indicando la severità del messaggio. Se questo è fatal, la connessione viene terminata, e nessuna connessione futura può essere stabilita per la sessione. Il secondo byte contiene il codice dell'alert. Il messaggio viene compresso e cifrato come un normale messaggio.

### Protocollo handshake
Permette l'autenticazione tra utente e server e viceversa, e di negoziare algoritmo di cifratura e MAC con le rispettive chiavi. Viene usato prima che qualunque altro dato sia trasmesso. Consiste in una serie di messaggi di diverso tipo (identificato dal primo byte) tra utente e server. 

In una prima fase, vengono stabilite le capacità di sicurezza (versione del protocollo, timestamp e valore casuale, identificativo sessione, lista ci cifrari e metodi di compressione).
Successivamente vengono scambiati certificati, chiavi e specifiche per gli algoritmi prima dal server all'utente (fase 2) e poi dall'utente al server (fase 3).
Infine, nell'ultima fase si conclude il setup della connessione e si inizializzano i cifrari.

## Parametri crittografici

I parametri usati da utente e server sono generati a partire da un **master secret** che le due parti si scambiano: per fare ciò, si generano hash sicure a partire dal secret per produrre i parametri di lunghezza corretta.

## Attacchi

Un attaccante può colpire:
- Il protocollo handshake. Si sfruttano la formattazione e l'implementazione di RSA.
- Il protocollo record e i protocolli sui dati da applicazione, principalmente attraverso attacchi chosen-plaintext.
- L'infrastruttura a chiave privata, in particolare, sulla validazione dei certificati X.509.
- Altro.