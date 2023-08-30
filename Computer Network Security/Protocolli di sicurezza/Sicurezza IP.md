Garantisce protezione per applicazioni che possiedono o non possiedono meccanismi di sicurezza, agendo a livello di networking.

La specifica di IPSec definisce:
- Architettura, che copre i concetti generali, le definizioni e i requisiti di sicurezza.
- Authentication Header, protocollo che specifica header di estensione per l'autenticazione e l'integrità dell'intero pacchetto, usando funzioni di hash con chiave.
- Encapsulating Security Payload, protocollo per cifratura ed integrità del payload.
- Internet Key Exchange, insieme di protocolli che descrivono la gestione delle chiavi da usare per IPSec.

### Applicazioni di IPSec
- Rete virtuale privata (VPN) sicura su Internet o una WAN pubblica.
- Accesso remoto sicuro attraverso Internet.
- Assicurare comunicazione con altre organizzazioni tramite autenticazione.
- Migliorare la sicurezza dell'e-commerce. 

## Funzionamento

Ad ogni pacchetto IP viene applicata una policy di sicurezza, determinata dall'interazione di due database: il **Security Association Database (SAD)** e il **Security Policy Database (SPD)**.

Un'**associazione di sicurezza (SA)** è una connessione logica unidirezionale tra mittente e destinatario. In ogni pacchetto l'SA è identificata dall'indirizzo del destinatario e da un parametro presente nell'header di estensione associato (AH o ESP). Tramite SA le due parti definiscono elementi comuni come algoritmo e chiavi.

### Security Association Database
Definisce i parametri associati ad ogni SA. Ogni entry comprende una lista di parametri, tra cui il **security parameter index** (presente nell'header) ed altri valori specifici.

### Security Policy Database
Ogni entry definisce un sottoinsieme di indirizzi IP che puntano ad una SA per il traffico.

## Servizi

### Encapsulating Security Payload
Provvede in insieme di header contenenti valori per la cifratura quali padding, lunghezza pad, numeri di sequenza, ecc.

Usato per cifrare il payload, il padding, la lunghezza del padding ed il campo Next Header, che specifica il tipo di contenuto. Se l'algoritmo necessità di IV, questi vengono esplicitati prima del payload.

Supporta anche integrità attraverso un servizio secondario. Applicando il controllo di integrità prima di decifrare il payload il processo viene facilitato e ottimizzato (se un pacchetto non è integro, non si procede alla decodifica). Si può anche procedere alla decifratura in parallelo al controllo di integrità.

### Internet Key Exchange
Permette di determinare e distribuire chiavi. Generalmente in una comunicazione si usano quattro chiavi: una coppia per trasmissione e ricezione per garantire integrità e confidenzialità.
La gestione può essere **manuale**, svolta da un amministratore, o **automatica**, che permette di creare chiavi on demand e facilitare l'uso delle chiavi su larga scala.

Il protocollo di gestione automatica delle chiavi usato da IPSec è **ISAKMP/Oakley**, che consiste in:
- **Internet Security Association and Key Management Protocol (ISAKMP)** per la gestione delle chiavi e per i formati di queste. Consiste in un insieme di tipi di messaggio che abilità l'uso di diversi algoritmi per lo scambio di chiavi.
- Un protocollo per scambiare le chiavi usando Diffie-Hellman con sicurezza aggiunta (Oakley).

## Modalità di trasporto

### Transport Mode
Alla sorgente i dati e gli header/trailer ESP, insieme al segmento del livello di trasporto, vengono cifrati ed il plaintext del blocco viene rimpiazzato con il corrispondente ciphertext per formare il pacchetto IP (con l'aggiunta dell'autenticazione se necessario).

Il pacchetto viene inviato a destinazione, e ogni router intermediario esamina solo l'IP per indirizzarlo.

Il destinatario esamina gli header IP ed in base agli header SPI/ESP decifra il resto del pacchetto per ottenere il segmento. 

Questa modalità garantisce confidenzialità anche se questa non viene implementata a livello applicativo. È tuttavia possibile effettuare analisi del traffico sui pacchetti.

### Tunnel Mode

Diversamente dalla Transport Mode, tutto il pacchetto IP viene protetto. Dopo aver aggiunto gli header AH e ESP l'intero pacchetto, headers inclusi, vengono trattati come il payload di un nuovo pacchetto IP con un header IP esterno. 
Nessun router lungo il tragitto può esaminare l'IP del pacchetto interno, che non è necessariamente lo stesso del pacchetto esterno.

Tunnel Mode viene usata quando entrambe le parti di una SA sono gateway di sicurezza, come un firewall o un router che implementa IPSec.
La modalità permette di far comunicare reti protette da firewall tra di loro in modo sicuro, senza che queste implementino IPSec.

Tunnel Mode può essere usata per implementare una **rete privata virtuale (VPN)**: è una rete privata configurata all'interno di una rete pubblica.

