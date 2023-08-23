Le chiavi sono tanto sicure quanto protette: generazione, persistenza, rimpiazzo e distribuzione devono essere sicuri.
Occorre inoltre controllare gli utilizzi delle chiavi ed il contesto di utilizzo.

Le chiavi dovrebbero essere cambiate frequentemente per evitare di compromettere grandi quantità di dati in caso una venga scoperta: occorre quindi un metodo efficiente e sicuro per la distribuzione di queste.

La distribuzione può essere fatta nei seguenti modi:
1. Le due parti si scambiano la chiave fisicamente.
2. Si usa una chiave condivisa in precedenza per cifrare la nuova chiave che la rimpiazza.
3. Una terza parte sceglie la chiave e la condivide fisicamente alle parti comunicanti.
4. Una terza parte dispone di una connessione sicura con le parti comunicanti e invia la chiave tramite essa.

Lo scambio con terza parte può avvenire tramite:
- **Key Translation Center**: una parte invia la chiave di sessione cifrata con la sua chiave alla terza parte, che la cifra con la chiave dell'altra parte prima di inviargliela. La terza parte potrebbe delegare l'invio della chiave cifrata del ricevente al richiedente stesso.
- **Key Distribution center**: la terza parte genera la chiave di sessione e la distribuisce.

### Scambio tra pari
Le due parti non possono richiedere direttamente la chiave di sessione l'una con l'altra in quanto, anche se si identificano, un attaccante può intercettare la comunicazione e fingersi una delle due (o entrambe le) parti.

Se invece la comunicazione permette di autenticare le parti e di evitare attacchi replay, le due parti possono scambiare la chiave in sicurezza: le due parti si autenticano comunicando il proprio identificativo cifrato con la chiave pubblica dell'altra, ed inviando una **sfida** consistente nel decidere un numero di sequenza, che deve essere corretto rispetto ai messaggi precedenti.
Le due parti riescono nella sfida solo se possono decifrare il messaggio, e quindi se sono i reali possessori della chiave necessaria.

### Public Key Directory

Una terza parte (**autorità**) mantiene una cartella con coppie di **identificativi** e **chiavi pubbliche** per ogni partecipante alla rete.
Ogni utente si registra in modo sicuro (fisicamente o attraverso una connessione autenticata), e può rimpiazzare la sua chiave in caso di necessità.

Lo scambio di chiavi di sessione avviene attraverso l'autorità. Si mantiene la sfida tra i due utenti.
Una volta terminato lo scambio, per cui sono necessarie 7 comunicazioni tra autorità e parti, i due utenti possono salvare la chiave pubblica dell'altra parte ed evitare l'overhead.

## Certificati

È un "blocco", contente chiave pubblica e identificativo, firmato da una terza parte fidata.
In questo schema, solo l'autorità può generare certificati.

Ogni certificato può essere letto da chiunque all'interno della rete, e può verificare che è ha origine dall'autorità certificante.

### Certificati X.509
Definisce delle linee guida per l'autenticazione ai servizi attraverso certificati.

Lo schema opera come segue:
1. Si effettua l'hash del **certificato non firmato**, tra cui l'identificativo dell'utente da certificare, la sua chiave pubblica ed altre informazioni su autorità ed il certificato.
2. Si cifra l'hash con la chiave privata dell'autorità per generare la firma. La firma concatenata al certificato non firmato, ottenendo il **certificato firmato**.
3. La verifica del certificato si effettua facendo l'hash del certificato senza firma e dandola in input all'algoritmo di verifica insieme a firma e chiave pubblica dell'autorità.