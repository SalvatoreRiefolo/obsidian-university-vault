Idea: usare termini come feature per dividere i dati in uno spazio t-dimensionale (t = dimensione vocabolario).

Nell'equazione del piano (o retta) che divide i dati $t_1, t_2, \dots$ sono i termini.
SVM trova due vettori di supporto per creare spazio in cui i punti non toccano il decision boundary.

La norma del vettore vettore W (perpendicolare tra i due supporti) è la distanza tra i due vettori di supporto.

L'obbiettivo delle SVM è massimizzare la distanza W rispetto al criterio di classificazione (1 e -1).

De tecniche: soft margin & kernel trick.

- Soft margin: penalità per ogni punto che la funzione non classifica correttamente, scalati tramite termine C.
- Kernel trick: si mappano punti in spazi con dimensionalità più alta e si vuole calcolare la similarità tra i punti: la matrice delle features però aumenta molto. Con il kernel trick si usano solo le features di partenza. Funzione di Kernel più intuitiva è la Gaussiana. Es: n documenti usati come punti di training per SVM (landmarks), si calcola la similarità tra punti e landmarks.

## SVM in IR

1. K-Means: clustering (unsupervised)
2. SVM (supervised)

### K-Means
Partiziona elementi in k classi. K random, inizializzati k centroidi e per ogni documento si calcola qual è il centroide più vicino. Successivamente per ciascuna coordinata dei cluster viene calcolato il punto centrale (che diventa il nuovo centroide). Dopo di che si effettua nuovamente la valutazione con il nuovo centroide.

### Approccio combinato: documenti
1. Processing testo $\Rightarrow$ rappresentazione vettoriale dei documenti usando termini stemmati.
2. Clustering con k-means.
3. Classificazione dei termini in *decisivi*, *triviali* o *standard*. Solo decisivi e standard sono usati per indicare le classi.

### Approccio combinato: query
Associare la query al cluster più adatto. Per ciascun cluster uso una SVM diversa (classificazione binaria, probabilità di appartenenza o meno ad un cluster). Si seleziona la probabilità più alta per decidere il cluster di appartenenza.