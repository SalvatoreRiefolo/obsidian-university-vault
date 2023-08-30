Il processo di determinare se un utente (o applicazione) sia chi dichiara di essere. Permette di gestire il controllo dell'accesso alle risorse di un sistema.

### Metodi di autenticazione

- Qualcosa di **conosciuto** dall'utente, come una password o un PIN. Possono essere condivise e dimenticate.
- Qualcosa di **posseduto** dall'utente, come un badge o un token hardware. Possono essere condivise, perse o duplicate.
- Qualcosa di **inerente** all'utente, come l'impronta digitale o la voce. Non possono essere condivise, ma possono generare falsi positivi e negativi.

Un **autenticazione a fattori multipli** richiede due o più metodi di autenticazione per l'accesso ad una risorsa. Tutti i controlli devono passare per guadagnare l'accesso.

### Autenticazione mutuale

Entrambe le parti devono essere autenticate con l'altra prima di poter inviare, sempre in via confidenziale, una chiave di sessione. 

Il **tempismo** è importante, in quanto il rimando di un messaggio (**replay**) da parte di un attaccante potrebbe portare ad attacchi di tipo **masquerade**: se l'attaccante riesce a rimandare il messaggio nella finestra temporale corretta, potrebbe farsi passare per una delle due parti.

Per evitare attacchi replay può usare: 
- Un **numero di sequenza** per ogni messaggio. Questo viene accettato solo se il numero è corretto all'interno della sequenza.
- Un **timestamp**, ma ciò richiede che i clock delle due parti siano sincronizzati. Un messaggio viene accettato se è "abbastanza vicino" al timestamp previsto.
- Una **sfida con risposta** (challenge/response) in cui il mittente aggiunge una nonce al messaggio e si aspetta una risposta contenente il valore di nonce corretto.

## Autenticazione remota con cifratura simmetrica

La strategia sfrutta un **centro di distribuzione delle chiavi (KDC)** che condivide una chiave (master key) con gli utenti, attraverso cui può cifrare e distribuire chiavi simmetriche agli utenti che desiderano comunicare tra loro.

### Protocollo di Needham-Schroeder

Il protocollo opera come segue:
1. $A$ invia al $KDC$ il messaggio $ID_{A}||ID_B||R_A$.
2. $KDC$ invia ad $A$ $E_{KA}(K_S||ID_B||R_A||E_{KB}(K_S||ID_A))$. $K_S$ è la chiave di sessione.
3. $A$ invia a $B$ $E_{KB}(K_S||ID_A))$ ricevuto nel messaggio precedente.
4. $B$ invia ad $A$ $E_{KS}(R_B)$. È la sfida che $A$ deve superare.
5. $A$ invia a $B$ $E_{KS}(f(R_B))$. La funzione applicata a $R_B$ è la risposta alla sfida.

Se una vecchia chiave di sessione è stata compromessa, il protocollo è vulnerabile allo step 3.: il messaggio può essere rimandato ed il messaggio dello step 4. intercettato. Questo messaggio può essere decifrato con la vecchia chiave e successivamente l'attaccante impersona il mittente, dando al destinatario una nuova chiave di sessione.
Il problema è risolvibile sando una nonce aggiuntiva o i timestamp, come dimostrato dal protocollo **Denning-Sacco**.

## Kerberos

Gli utenti potrebbero ottenere accesso a dispositivi non propri e richiedere accesso a server per servizi che non sono autorizzati ad accedere, impersonando il reale proprietario/utente del dispositivo.

Il protocollo prevede un server di autenticazione centralizzato che autentica server per gli utenti e gli utenti per i server. Viene usata esclusivamente la cifratura a chiave simmetrica (DES) per la comunicazione.

### Componenti

Il protocollo è composto da 3 componenti:
1. **Authentication server (AS)**: dispone di un database con le password di tutti gli utenti, e condivide un'unica chiave privata con ogni server. Consegna all'utente autenticato un **Ticket-granting ticket**, con cui l'utente può autenticarsi verso il Ticket-granting server per richiedere accesso ad un particolare servizio.
2. **Ticket**: viene creato quando l'AS accerta che l'utente è autentico, e contiene informazioni sull'utente e sul server con cui vuole comunicare. Viene cifrato usando la chiave condivisa con il server. 
3. **Ticket-granting server (TGS)**: procura i ticket agli utenti autenticati dall'AS. Quando un utente vuole richiedere accesso ad un nuovo servizio richiede il ticket al TGS per autenticarsi; il TGS consegna il ticket per il servizio, e l'utente lo salva per autenticarsi con il server quando richiede tale servizio. Così facendo, l'utente non deve autenticarsi ogni volta che vuole accedere ad un servizio diverso.

## One-Way Authentication

L'informazione ha una sola direzione, da un mittente ad un destinatario
Nella forma più semplice, viene stabilità l'identità delle parti e si usa qualche forma di token di autenticazione per certificare che il messaggio origina dal mittente ed è destinato al destinatario.

Si può usare una firma digitale per garantire l'autenticazione e e la cifratura simmetrica per la confidenzialità.

## OAuth 2

Permette alle applicazioni web di richiedere accesso (ristretto) all'account di un'altra applicazione.
L'utente può così non rivelare le proprie credenziali all'applicazione a cui vuole accedere.

