Dato un insieme di categorie, determinare a quali categorie appartengono degli oggetti.
L'obiettivo è raggruppare gli elementi simili nello stesso gruppo, anche in situazioni in cui i gruppi non siano precedentemente definiti.

## Classificatori

Si utilizzano tecniche di supervised learning per addestrare un modello su un dataset annotato, che poi utilizza le features imparate per classificare correttamente gli oggetti.

Esistono diversi classificatori che si basano anche su tecniche di machine learning.

### Classificatore di Bayes
Simile al [[3. Modello probabilistico|modello probabilistico]], calcola la probabilità che un documento $d$ appartenga ad una categoria $c$.

$$p(c|d) = p(c) \prod_{i=1}^n p(k_i | c)$$

dove:
- $p(c)$ è il rapporto tra documenti nel cluster $c$ ed il numero di documenti totali.
- $p(k_i, c)$ è il rapporto tra il numero di occorrenze di $k_i$ in $c$ ed il numero totale di occorrenze in $c$.

Presenta due problemi fondamentali:
1. I nuovi documenti su cui il modello non è stato addestrato potrebbero contenere termini "misti" tra i diversi cluster.
2. I documenti potrebbero contenere termini nuovi, ed il classificatore darebbe 0 come risultato.

Per questi motivi si applica lo **smoothing**, aggiungendo un piccolo numero per rimuovere gli zero.

## Cluster

Un gruppo di elementi. Ogni cluster ha un **centroide**, la media degli elementi del cluster. Si possono definire anche **ipercentroidi** tra cluster multipli (la media dei centroidi dei cluster).

Gli elementi sono raggruppati in base alla loro similarità: non esistono categorie predefinite. Il clustering permette di creare delle categorie a partire dalla similarità degli elementi di una collezione.

In IR, il clustering può essere usato per:
- Raggruppare la **collezione**: l'utente può facilmente esplorare i cluster.
- Raggruppare i **documenti** simili per mostrare i risultati in maniera efficace.
- Raggruppare i **termini** per favorire la riformulazione delle query.

## Algoritmi di clustering

### Clustering gerarchico
Un cluster può contenere:
- Elementi, se abbastanza piccolo.
- Elementi ed altri cluster se grande abbastanza.

I cluster possono formare cluster a loro volta: si crea così un albero.

Una **gerarchia di cluster** raggruppa gli elementi in **livelli di similarità**. I livelli vicini alle foglie sono i cluster più piccoli, mentre quelli vicini alla radice sono quelli più grandi.

Un cluster gerarchico può essere creato:
- **Top down**: si parte dalla radice che contiene tutti gli elementi. Successivamente viene partizionata separando gli elementi dissimili e raggruppando quelli simili.
- **Bottom up**: si raggruppano i documenti in cluster e si creano cluster più grandi con i cluster simili, fino a che non si racchiude l'intera collezione. È il metodo più usato.

Per raggruppare i cluster si usano tre approcci:
- **Single link**: la similarità tra due cluster è la similarità **massima** tra gli elementi dei cluster. Si minimizza la "distanza" tra i cluster, si uniscono quindi i gruppi "più vicini" e privilegia i cluster sparsi.
- **Complete link**: la similarità tra due cluster è la similarità **minima** tra gli elementi dei cluster. Si massimizza la **distanza** tra i cluster, si uniscono quindi i gruppi "meno lontani" e privilegia i cluster piccoli e compatti.
- **Group link**: la similarità tra due cluster è la similarità **media** tra gli elementi dei cluster. Si può calcolare anche sui centroidi.

Si sceglie sempre il valore minimo trovato per scegliere quali cluster raggruppare.

### Calcolo della similarità
Si usa la matrice $M$ che contiene i documenti ed i loro termini. La matrice $M \cdot M^T$ è la **matrice di similarità** tra i documenti: si suppone che i i documenti contenenti gli stessi termini siano **semanticamente simili**. 

Per grandi collezioni, il metodo è computazionalmente troppo costoso ($O(N^2)$). Si usano altre funzioni più efficienti per il calcolo della similarità.


### One-pass clustering

Si pone il primo documento nel primo cluster. Dopo di che, si considera il documenti e si compara la somiglianza con il primo: se abbastanza simile (rispetto ad una certa soglia) si mette nel cluster col primo documento, altrimenti si crea un secondo cluster. Per ogni nuovo documento, si compara la similarità col centroide dei cluster esistenti, e si aggiunge ad essi o crea un nuovo cluster in base alla soglia.

I cluster ottenuti dipendono dall'ordine dei documenti: ordinamenti diverso generano cluster diversi. La soglia potrebbe causare cluster troppo piccoli o grandi: tecniche di divisione o fusione possono essere usate per i cluster di questo tipo.

Questo metodo non opera molto meglio del cluster gerarchico.

### Rocchio clustering

Si basa sul numero di vicini nello spazio dei documenti (**densità**). 
Si prende un documento alla volta. Se il documenti ha una densità abbastanza alta (ha tanti vicini) e non è in alcun cluster, si genera un cluster a cui si uniscono il documento e tutto il suo vicinato.