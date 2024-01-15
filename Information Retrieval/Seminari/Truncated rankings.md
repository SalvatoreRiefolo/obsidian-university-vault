---
tags: 
  - information-retrieval
---

L'idea è di limitare il numero di documenti recuperati su cui effettuare il ranking.

Metriche come Average Precision e nDCG non sono buoni indicatori per stabilire la bontà dei risultati quando molti documenti irrilevanti sono recuperati "in coda". Queste metriche variano poco in questi casi e non permettono di valutare la qualità dei risultati.

### Proprietà delle metriche
Ogni metrica per il ranking dovrebbe avere le seguenti proprietà:
- **Priorità**: documenti rilevanti per primi.
- **Top-weightedness**: documenti rilevanti con rank più alto valgono di più.
- **Deepness threshold**: area (dei documenti recuperati) mai raggiunta dall'utente.
- **Shallowness threshold**: area sempre raggiunta dall'utente.

Per il ranking troncato si introducono 3 proprietà:
- **Confidenza**: avere documenti non rilevanti in coda alla lista dei ranking **diminuisce** il punteggio di efficacia.
- **Recall**: il numero di documenti rilevanti nella collezione impatta il punteggio. Si impone sensibilità rispetto al **punto di troncatura** (idealmente si vogliono recuperare tutti i documenti rilevanti).
- **Ridondanza**: un documento dovrebbe avere più peso se è il primo documento che l'utente trova. Proprietà utile se l'utente cerca un solo documento rilevante.