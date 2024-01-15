---
tags: 
  - information-retrieval
---

Si definiscono i **topic**, a cui sono associate parole (ogni parola appartiene ad un topic). Un documento tratta più topic diversi: queste sono variabili **latenti** (nascoste) del documento.

L'obiettivo è trovare pattern latenti in un insieme documenti usando le distribuzioni statistiche dei termini da usare nel recupero e nel clustering senza supervisione. I topic vengono creati automaticamente: il numero di topics $k$ è un parametro controllabile.

## Bag of Words
Un documento è rappresentato attraverso una lista dei suoi termini associati al numero di occorrenze. Dati due documenti, la loro similarità è data dalla similarità delle loro bag of words, rappresentabile attraverso la matrice di similarità coseno $M \times M^T$, con $M$ la matrice documenti-termini.

## Latent Semantic Indexing

Affrontato in [[5. LSI (Latent Semantic Indexing)]]. Riduce la matrice delle occorrenze documento-termine in una matrice che mette in relazione documenti a topics.

Il numero dei topics è arbitrario e non ha base statistica e utilizza i pesi assegnati da [[2. TF.IDF|TF.IDF]].

## pLSI: Probabilistic LSI

Si utilizzano distribuzioni **multinomiali**, cioè sequenze di $n$ tentativi con multipli possibili risultati.

pLSI utilizza ancora $k$ come parametro, ma si basa sulle probabilità invece che su Single Value Decomposition. 

Le probabilità vengono applicate su topic e parole ma non sui documenti.

## Latent Dirichlet Allocation

Famiglia di distribuzioni di probabilità che generalizza le distribuzioni beta usando un numero arbitrario di parametri.

LDA estende BoW sui documenti usando due vettori di parametri: uno indica la distribuzione **documento-topic** (valori alti indicano che un documento copre molti topic), l'altro la distribuzione **topic-parola** (valori alti indicano che ogni topic contiene molte parole rilevanti).

