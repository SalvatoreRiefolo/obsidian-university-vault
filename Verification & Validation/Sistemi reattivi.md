Un **sistema reattivo** reagisce ad **eventi** ed opera **per sempre**.
Alcuni esempi di sistemi reattivi sono i sistemi operativi, le stampanti, sistemi real-time di monitoraggio e simili.

Il sistema dovrebbe essere in grado di reagire a tutti i tipi gli eventi che l'ambiente presenta e a tutti i suoi **comportamenti**, anche quelli di "errore" devono essere gestiti.
Tale sistema è **corretto da costruzione**: se la specifica copre tutti gli scenari possiamo creare automaticamente il sistema dalla specifica.

### Concorrenza
Le componenti del sistema sono **concorrenti** fra loro e con l'ambiente.
La comunicazione tra le componenti può avvenire tramite **passaggio di messaggi** o **variabili condivise**.

Facendo eseguire due processi allo stesso tempo otteniamo **true concurrency**. Se invece si eseguono azioni atomiche dei due processi alternatamente, si parla di **interleaving**.

### Modelli 
I modelli considerati per i sistemi reattivi sono i **fair transition system** e gli **automi**, ed il linguaggio usato è la **logica temporale**, che garantiscono:
- **Safety**: i casi negativi non occorrono **mai** ($\forall$)
- **Liveness**: i casi positivi occorrono prima o poi ($\exists$)


