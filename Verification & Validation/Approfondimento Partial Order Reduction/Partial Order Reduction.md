Tecnica per ridurre lo spazio degli stati esplorato da algoritmi di model checking che sfrutta l'indipendenza tra transizioni concorrenti. Permette di effettuare model checking basato sui rappresentati, questa tecnica è utile per sistemi asincroni per cui non è richiesta sincronizzazione tra le componenti.

Le transazioni indipendenti possono essere eseguite in qualunque ordine, ma il model checking classico considera tutti i possibili ordinamenti, causando potenzialmente il problema dell'esplosione degli stati.
Se le transizioni sono indipendenti, tutte le possibili sequenze portano allo stesso stato. 

Con $n$ transizioni indipendenti ci sono:
- $n!$ possibili ordinamenti.
- $2^n$ stati differenti.

Se si scegliesse solo una sequenza rappresentativa tra quelle indipendenti il numero di stati si riduce a $n + 1$.

### Grafo degli stati ridotto

Anziché costruire il grafo completo per poi ridurlo, si costruisce un grafo degli stati ridotto che cattura un sottoinsieme di comportamenti del grafo completo che rappresenta tutti i possibili comportamenti di questo: se un comportamento non è presente nel grafo ridotto non è presente nemmeno in quello completo.

Per far ciò, per ogni classe di equivalenza dei comportamenti del grafo completo si selezionano dei **rappresentati** da tenere nel grafo ridotto. La relazione considerata è la **relazione di dipendenza**.

L'obiettivo è ridurre il numero di stati esaminati da model checking, preservando correttezza e completezza. Il grafo ridotto viene generato usando una versione modificata della visita DFS, che termina con risposta positiva se la proprietà è soddisfatta nel grafo completo, altrimenti produce un controesempio.

### Struttura di Kripke modificata

Si definisce come **sistema di transizioni di stato** $M$ una quadrupla

$$M = (S, T, S_0, L)$$

dove:
- $S$ è  l'insieme degli stati.
- $S_0$ è l'insieme degli stati iniziali.
- $L$ è la funzione di labeling che assegna ad ogni stato le proposizioni atomiche in esso vere.
- $T$  è un insieme di transazioni tale che $\forall \alpha \in T, \alpha \subseteq S \times S$.

Una transizione $\alpha$ è **abilitata** in $s$ se esiste $s'$ tale che $\alpha(s,s')$, altrimenti è **disabilitata** in $s$. Si denota con $\text{enabled}(s)$.

Una transizione è **deterministica** se $\forall s \exists! s' : \alpha(s,s')$. Ogni stato può raggiungere al più un altro stato attraverso la transazione $\alpha$.

Un **cammino** $\pi$ da $s_0$ è una sequenza finita o finita $\pi = s_0 \xrightarrow{\alpha_0} s_1 \xrightarrow{\alpha_1}\dots$ tale he $\forall i, \alpha_i(s_i, s_{i+1})$

## Proprietà delle transizioni

### Indipendenza

Una **relazione di indipendenza** $I \subseteq T \times T$ è una relazione: 
- simmetrica: $\forall \alpha, \beta$ se $\alpha$ è in relazione con $\beta$ anche $\beta$ è in relazione con $\alpha$.
- antiriflessiva: $\forall \alpha \in I$, $\alpha$ non è in relazione con se stesso.

$\forall s \in S$ e $\forall (\alpha, \beta) \in I$, valgono due condizioni:
- **Enabledness**: se $\alpha, \beta \in \text{enabled}(s)$, allora $\alpha \in \text{enabled}(\beta(s))$. Se due transizioni sono abilitate, se si prende una delle due l'altra è ancora abilitata.
- **Commutatività**: se $\alpha, \beta \in \text{enabled}(s)$, allora $\alpha(\beta(s)) = \beta(\alpha(s))$. Indipendentemente dall'ordine in cui si prendono le transizioni, lo stato raggiunto è lo stesso. 

Anche quando due transizioni sono indipendenti e la commutatività è rispettata, generalmente non è possibile rimuovere una delle due transizioni. La proprietà da verificare potrebbe essere sensibile alla scelta di uno dei due stati intermedi o questi potrebbero avere altri successori che potrebbero risultare non più raggiungibili.

In caso sia difficile verificare che due transizioni siano indipendenti si assume che queste non lo siano per mantenere la correttezza della riduzione.

### Invisibilità

Dati:
- $AP$ insieme delle proposizioni atomiche.
- $L: S \rightarrow 2^{AP}$ funzione di labeling.

Una transizione $\alpha \in T$ è **invisibile** rispetto a $AP' \subseteq AP$ se $\forall s, s' \in S : \alpha(s, s')$, $L(s) \cap AP' = L(s') \cap AP'$.

Se l'esecuzione di una transizione in qualunque stato in cui è abilitata non cambia il valore delle variabili in $AP'$ essa è invisibile rispetto ad $AP'$.

## Stuttering

Dati due cammini infiniti $\sigma$ e $\rho$ questi sono equivalenti rispetto allo stuttering, denotato $\sigma \sim_{st} \rho$ se esistono due sequenza infinite di interi positivi $i, j$ per $\sigma, \rho$ tali che le label di ogni stato negli intervalli definiti dagli indici siano uguali.

A parità di numero di stati con lo stesso labeling, i due cammini sono equivalenti.

Una formula LTL $f$ è invariante rispetto allo stuttering se per ogni coppia di cammini $\pi, \pi'$ t.c. $\pi \sim_{st} pi'$: 

$$\pi \models f \leftrightarrow \pi' \models f$$

### LTL$_{-X}$

Frammento di LTL privo di operatore **Next**. Ogni formula LTL$_{-X}$ è invariante rispetto allo stuttering ed ogni formula LTL invariante rispetto allo stuttering può essere espressa in LTL$_{-X}$.

### Stuttering sulle strutture

Due strutture di Kripke $M, M'$ sono equivalenti rispetto allo stuttering se e solo se:

- $M, M'$ hanno gli stessi stati iniziali.
- $\forall \sigma \in M$ che parte da uno stato iniziale $s$ esiste un cammino $\sigma' \in M'$ che parte da $s$ t.c $\sigma \sim_{st} \sigma'$. Simmetrico per $M'$.

Date $M, M'$ equivalenti rispetto allo stuttering, per ogni formula $f$ in LTL$_{-X}$ e per ogni stato iniziale $s \in S_0$:

$$M, s \models f \leftrightarrow M', s \models f$$

Il grafo prodotto con partial order reduction è equivalente rispetto allo stuttering al grafo completo.

## Generazione del grafo ridotto

La visita DFS modificata utilizza una procedura per calcolare l'insieme $\text{ample}(s)$, sottoinsieme di $\text{enabled}(s)$. In ogni stato si considerano un sottoinsieme delle transizioni tali che sia possibile assicurare la correttezza dei risultati considerando un numero ridotto di comportamenti, riducendo allo stesso tempo il numero di stati con un costo ragionevole. Il grafo viene creato rispetto ad un sottoinsieme $AP' \subseteq AP$.

Una formula invariante rispetto allo stuttering, commutatività e invisibilità consente di evitare la generazione di alcuni stati.

Gli **ample set** generati attraverso la prodecura $\text{ample}(s)$ nella visita DFS modificata sono usati per far si che per ogni cammino escluso ne venga considerato uno equivalente rispetto allo stuttering.

## Condizioni degli ample sets

Perché la soddisfacibilità di una formula LTL$_{-X}$ venga preservata si introducono 4 condizioni  per selezionare $\text{ample}(s)$. Se una delle condizioni viene violate si considera l'intero insieme delle transizioni abilitate in $s$, $\text{enabled}(s)$.

### C0: presenza del successore

Se uno stato $s$ ha almeno un successore nel grafo completo ha almeno un successore nel grafo ridotto.

$$\text{ample}(s) = \varnothing \leftrightarrow \text{enabled}(s) = \varnothing$$

### C1: dipendenza

Per ogni cammino del grafo che parte da $s$ nel grafo completo, una transizione che dipende da una transizione in $\text{ample}(s)$ non può essere eseguita senza che una transizione in $\text{ample}(s)$ venga eseguita prima.

Segue che le transizioni in $\text{enabled}(s) \setminus \text{ample}(s)$ sono indipendenti da quelle in $\text{ample}(s)$.

I cammini del grafo completo non presenti nel grafo ridotto hanno una delle due seguenti forme: 

1. Il cammino ha un prefisso $\beta_0\beta_1\dots\beta_m \alpha$ con $\alpha \in \text{ample}(s)$ e ogni $\beta_i$ è indipendente in tutte le transizioni in $\text{ample}(s)$. Questo implica che $\beta_i \notin \text{ample}(s)$.
2. Il cammino è una sequenza infinita $\beta_0\beta_1\dots$ con $\beta_i$ indipendente da tutte le transizioni in $\text{ample}(s)$.

### C2: invisibilità

Se $s$ non è fully expanded ($\text{ample}(s) \neq \text{enabled} (s)$), allora ogni transizione $\alpha \in ample(s)$ è invisibile.

### C3: aciclicità

Non è consentito avere un ciclo che contiene uno stato in cui una transizione $\alpha$ è abilitata e non inclusa in $\text{ample}(s)$ per ogni stato $s$ del ciclo.

### Verifica del rispetto delle condizioni

### Calcolo degli ample sets