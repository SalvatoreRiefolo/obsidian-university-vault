---
tags: 
  - computer-network-security
---

Esistono due famiglie di protocolli per trasferire email:
1. **Simple Mail Transfer Protocol (SMTP)** viene usato per trasportare messaggi da mittente a destinatario.
2. **Internet Mail Access Protocol (IMAP) e Post Office Protocol (POP)** sono usati per trasferire messaggio tra server email. I server si autenticano tra loro e inviano comandi per lo scambio di email.

Il formato per i messaggi testuali usando email è definito da **RFC 5322**: i messaggi sono visti come **busta (envelope)**, che contiene le informazioni per la trasmissione, e **contenuto (content)**, l'oggetto da inviare.

## MIME

SMTP può inviare solamente dati testuali in certi formati, e non è in grado di trasmettere file eseguibili o binari.

**Multipurpose Internet Email Extension (MIME)** è un'estensione di RFC 5322 per ovviare alle limitazioni di SMTP.
Definisce header aggiuntivi, formati di contenuto supportati e codifiche per il trasferimento.

### Headers
Tra gli header si trovano:
- MIME-Version, che indica se il messaggio è conforme.
- Content-Type, che descrive il tipo di contenuto del messaggio.
- Content-Transfer-Encoding, la trasformazione applicata al contenuto prima di essere inviato.
- Content-ID per identificare entità MIME univocamente in contesti multipli.
- Content-Description.

### Tipi di contenuto
MIME supporta vari tipi di contenuto:
- Diversi tipi di body testuale (plain, multipart con rispettivi sotto-tipi).
- Tipo di messaggio.
- Diversi formati di vari contenuti multimediali (immagini, video, audio).
- Applicativi e binari.

### Encodings

I dati possono essere rappresentati in ASCII, binario, base64 o in formato stampabile, o con un formato non standard ("**x-token**").

## Minacce alla sicurezza email

Possono essere divise in:
- Minacce all'autenticità
- Minacce all'integrità
- Minacce alla confidenzialità
- Minacce alla disponibilità

Sono stati creati una serie di protocolli come mezzo per contrastare queste minacce.

### S/MIME
**Secure MIME** garantisce quattro servizi relativi al messaggio: 
- autenticità, usando firme digitali generate con RSA/SHA.
- confidenzialità, cifrando con AES in modalità CBC.
- compressione.
- compatibilità, per garantire trasparenza per applicazioni email.

## Domain Name System (DNS)
Un servizio di lookup che mappa i nomi degli host con il loro indirizzo IP. Necessario per determinare il next hop per la consegna di email.

È costituito da:
- **Domain name space**, una struttura ad albero per identificare le risorse.
- Un **database** contente record, ognuno rappresentante nome, IP e altre informazioni degli host. Questo database è distribuito ed è **gerarchico**.
- **Name servers**, programmi che mantengono informazioni sull'albero e sui record associati.
- **Resolvers**, programmi che estraggono le informazioni dai name servers in risposta alle richieste.

### DNSSEC
Estensione di sicurezza di DNS. Garantisce protezione end-to-end usando firme digitali create dall'amministratore di una zona. Tramite questo protocollo non è necessario che i server intermedi che reindirizzano o memorizzano i record DNS siano fidati.
L'obiettivo finale è dunque proteggere gli utenti dall'accettare record DNS fasulli o alterati.

### DANE
**DNS-based Authentication of Named Entities (DANE)** è un protocollo che permette di collegare certificati X.509 a nomi DNS usando DNSSEC. L'obiettivo è permette l'autenticazione tra client e server attraverso TLS senza il bisogno di un'autorità.