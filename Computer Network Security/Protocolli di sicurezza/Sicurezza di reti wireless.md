---
tags: 
  - computer-network-security
---

Una rete wireless presenta più rischi alla sicurezza di una tradizionale rete cablata:
- Il networking avviene solitamente con comunicazioni broadcast, molto più suscettibili ad eavesdropping.
- Possono essere portatili, permettendo accesso al di fuori di un perimetro stabilito.
- I dispositivi che usano reti wireless, come telefoni e tablet, possiedono risorse limitate e sono più suscettibili ad attacchi su di esse (DOS, malware).
- Alcuni dispositivi wireless potrebbero essere fisicamente accessibili da attaccanti.

Una comunicazione wireless avviene tra un **endpoint** (il client) ed un **access point** (che provvede connessione alla rete), attraverso un mezzo di trasmissione (l'aria).

Le principali minacce possono essere raggruppate in:
- **Eavesdropping**: mascherare il segnale (ad esempio disattivando il broadcasting SSID, o limitando il segnale) aiutano a combattere la minaccia, così come la cifratura.
- **Inserimento o alterazione dei messaggi**: un attaccante potrebbe intercettare messaggi in una comunicazione.
- **Interruzione del servizio**: un attaccante potrebbe interrompere il corretto funzionamento del servizio attraverso attacchi di tipo Denial of Service.

## IEEE 802.11

Standard sviluppato dal comitato IEEE 802 per proteggere wireless LAN (local area network).

Il protocollo è formato da tra layers:
- **Layer fisico**: include le funzioni per codifica/decodifica dei segnali, trasmissione dei bit e altre caratteristiche come antenne e bande di frequenza.
- **Media Access Control**: si occupa di ricevere blocchi dal livello superiore e di assemblare **frames** con indirizzi e campi di rilevamento errori, e di verificarli in ricezione.
- **Logical Link Control**: si occupa di rilevare gli errori e di inviare nuovamente i frames danneggiati. Nell'architettura del protocollo LAN, queste azioni vengo svolte direttamente dal Media Access Control, e LLC si occupa solo di tenere traccia dei frame inviati con successo e ritrasmessi.

### Wired Equivalent Privacy (WEP)
Algoritmo di sicurezza della prima versione di IEEE 802.11.
Utilizza cifratura simmetrica tra host e access point per offrire confidenzialità e restringere l'accesso solo agli host autorizzati. Solo i pacchetti autenticati possono accedere alla rete.

Si usano IV e chiave per generare una sequenza pseudocasuale da sommare al messaggio, cifrandolo; l'IV viene inviato tra gli header prima del payload all'interno del frame. 

Non ci sono obblighi sulla frequenza di cambio della chiave, che di solito rimangono invariate a lungo. L'IV è solitamente incrementato (dopo aver raggiunto il valore massimo, il contatore rincomincia da 0), o è generato casualmente (vulnerabile al paradosso del compleanno).

Inviando un numero di pacchetti sufficientemente alto è possibile trovare due pacchetti cifrati con la stessa coppia $(IV, key)$: sommando i due ciphertext le chiavi si annullano producendo la somma dei due plaintext. 
Recuperati i plaintext (attraverso crittoanalisi) è anche possibile anche recuperare la chiave, permettendo così ad un attaccante di comunicare liberamente.

### Wi-Fi Protected Access (WPA)

Nel tempo sono stati aggiunti diversi protocolli di protezione per ovviare a questi problemi introducendo autenticazione mutuale con scambio di chiave di sessione, disciplinando l'uso dell'IV e usando algoritmi di cifratura più robusti.

L'estensione di WEP che sfrutta questi protocolli è chiamata **Wi-Fi Protected Access**.

### Robust Security Network

Evoluzione di WPA, definisce tre servizi:

- **Autenticazione** attraverso AS che assegna chiavi temporanee.
- **Controllo dell'accesso** che forza l'uso dell'autenticazione, indirizza i messaggi correttamente e facilita lo scambio delle chiavi.
- **Privacy e integrità del messaggio** su dati di livello Media Access Control. Usa CCM e HMAC per fare ciò.