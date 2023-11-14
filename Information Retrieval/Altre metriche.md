La curva P/R rappresenta la precisione rispetto alla recall ad ogni documento recuperato. La precisione è rappresentata come funzione della recall.

La precisione può essere calcolata in modo esatto, la recall no in quanto la collezione non è completamente disponibile. 
Le due metriche sono importanti in misura diversa in base alla necessita e al tipo di sistema.

### Fallout
Proporzione di documenti irrilevanti recuperati rispetto ai documenti irrilevanti nella collezione.

$$F = \frac{i}{N - r}$$

con
- $i$ numero di documenti irrilevanti recuperati.
- $N$ numero di documenti totali.
- $r$ numero di documenti rilevanti totali.

### Fattore di generalità 
Proporzione di documenti rilevanti totali su tutti i documenti.

$$G = \frac{r}{N}$$

con
- $r$ numero di documenti rilevanti totali.
- $N$ numero di documenti totali.

### F-misura ed E-misura
$F$ è la media armonica di P ed R. $E$ è definita parametrizzando $F$. Queste misure rappresentano una media di precisione e recall.

### Expected Search Length
$ESL(N)$ è numero di documenti da esaminare prima di avere $N$ documenti rilevanti, in base al ranking. A differenza di P ed R non dipende dalla query. 
Viene calcolato sulla media delle query.

### Precisione media
Si fa la media dei valori di precisione ottenuti quando un documento viene classificato come rilevante: lo step in cui la recall aumenta nella curva. Rappresenta l'area sotto la curva P/R.

### Mean Average Precision (MAP)
La precisione media vale per una query, la MAP è la media di questi valori calcolata su più query.

### R-precisione
La precisione dopo aver recuperato $R$ documenti, con $R$ numero di documenti rilevanti nella collezione: se la query ha $R$ documenti rilevanti è la precisione dopo i primi $R$ documenti recuperati. Vale 1 se tutti i documenti recuperati sono rilevanti e 0 se nessuno è rilevante.

### P@N
Generalizzazione della R-precisione, la precisione dopo $N$ documenti recuperati. Nel caso della R-precisione $N=R$.

### Discounted Cumulative Gain (DCG)
I documenti più rilevanti dovrebbero essere recuperati nelle prime posizioni del ranking, perché il "guadagno" per l'utente è maggiore.
**DCG** misura il guadagno che un documento da all'utente *scontato* da $\log(r)$, con $r$ il rank. Solitamente si calcola la media tra più query.

La metrica mostra un guadagno maggiore per query con più documenti rilevanti rispetto a quelle con meno documenti rilevanti: per questo motivo si usa **NDCG**, la versione normalizzata.

### Sliding ratio
La rilevanza non è binaria ma è continua. Idealmente il sistema ordina i documenti e attribuisce il rank in base alla rilevanza. **Sliding ratio** misura la differenza tra un sistema ideale ed il sistema reale, per quanto riguarda la distanza tra ranking "perfetto" e ranking ottenuto.

### SRS e URS
Ranking di rilevanza attribuiti dal sistema attraverso metriche e rilevanza attribuita dall'utente.

Le due misure possono essere usate per indicare la distanza tra le due viste (del sistema e dell'utente) per comprendere su che documenti il sistema ha attribuito un ranking troppo alto o basso.

P ed R sono ipersensibili ed insensibili rispetto a variazioni in zone diverse della distribuzione rispetto ad una certa soglia.

### Average Distance Measure
ADM è $1 -$ la media della distanza tra SRS e URS rispetto a tutti i documenti. Per un sistema il valore è calcolato come media su tutte le query.

Si possono definire precisione e recall:

- $P$: ADM calcolato solo su documenti **sottovalutati**.
- $R$: ADM calcolato solo su documenti **sopravalutati**.