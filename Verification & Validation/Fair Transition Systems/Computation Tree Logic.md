Rispetto ad [[Logica Temporale|LTL]], gli stati possono avere più successori: il tempo si ramifica, cioè è possibile avere più modelli lineare allo stesso momento.

Un CTL è una tripla $M = (S, R, L)$, la cui struttura è simile a quella di LTL.
- $S$ è l'insieme di stati.
- $R \subset S \times S$ sono le relazioni tra gli stati.
- $L: S \rightarrow AP^{Q}$ una funzione di labeling.

Un CTL è rappresentabile tramite un albero, potenzialmente infinito se il grafo di esecuzione presenta cicli.

### Quantificatori

$X,U$ (next, until), sono chiamati **quantificatori di stato**.
$A,E$ (tutti, esiste), sono chiamati **quantificatori di computazione** o percorso (**path**).
In una formula CTL i quantificatori di stato e di computazione sono alternati.

Una formula CTL è un insieme di formule di stato. Il futuro si ramifica, ma il passato (solitamente) è lineare.

Ad esempio, la formula $AG(P \rightarrow AFQ)$ indica che ovunque nell'albero, se $P=true$ allora in tutti i futuri eventualmente $Q=true$.

La formula $EFP \rightarrow EF\,EGP$ indica che se esiste un futuro in cui eventualmente $P=true$, allora esiste un futuro in cui eventualmente $p=true$ sempre. I due rami non coincidono per forza.

### Formule CTL$^*$
I quantificatori di stato e di computazione non sono per forza alternati. Nel caso di due quantificatori di stato successivi l'uno all'altro, questi non possono fare riferimento allo stesso predicato $p$.

## Model Checking in CTL

LTL e CTL sono incomparabili in termini di espressività. LTL è un **frammento** di CTL.
Ci sono formule LTL che non possono essere convertite in CTL (es., $FGp$ ha due quantificatori di stato in successione) e formule CTL che non possono essere convertite in LTL ($EFQ$ ha senso solo se esistono percorsi infiniti).

Il model checking decide se $M, s \models \varphi$.
Si elaborano le sotto-formule e si valutano indipendentemente in maniera ricorsiva le formule del tipo $EU, AU$.

### Complessità
Siano:
- $n = |M|$ il numero di nodi.
- $m = |R|$ il numero di archi.
- $k$ la lunghezza della formula.

I casi booleani costano $O(n)$, i casi temporali costano $O(n+m)$ (si propaga ai nodi collegati). La procedura itera per $k$ volte: la complessità e quindi lineare rispetto al prodotto della lunghezza della formula e alla dimensione del modello.

## Problema dell'esplosione degli stati

Durante il model checking si potrebbe incorrere nel problema dell'**esplosione degli stati**: questo avviene quando si hanno molte componenti concorrenti.
La composizione di due componenti concorrenti è modellata attraverso il prodotto cartesiano dei rispettivi insiemi di stati. Una possibile soluzione è calcolare su insiemi di stati invece che su stati singoli, effettuando **model checking simbolico** usando OBDD.

### Model checking simbolico
Permette di manipolare le definizioni degli insiemi di stati, enumerando implicitamente un grande numero di stati.

Gli **stati** sono rappresentati tramite vettori di bit di lunghezza fissa $m$: $S \subseteq \{0,1\}^m$.
$R \subseteq S \times S$ è definita da una formula booleana $\mathcal{B}(x_1,\dots,x_m,x'_1,\dots,x'_m)$), dove $x_1,\dots,x_m$ è lo stato durante un certo istante e $x'_1,\dots,x'_m$ è lo stato all'istante successivo.

Si cerca di raggiungere uno stato $target$, definito da una formula. Attraverso $R$ si cerca di raggiungere lo stato $target$ dalla formula di partenza cercando di ottenere $EF \, target$ ricorsivamente.

Si usano gli [[Order Binary Decision Diagram]] per risolvere efficientemente il problema: usando la struttura dati, si riduce lo spazio di ricerca.