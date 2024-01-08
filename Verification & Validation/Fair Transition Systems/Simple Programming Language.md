È un linguaggio usato per sintetizzare programmi reattivi.

### Istruzioni
- **Skip**: nessun effetto sui dati ma fa avanzare il controllo
- **Assignment($u_1,\dots,u_k$) := ($e_1,\dots,e_k$)** assegna il termine/espressione $e_i$ alla variabile $u_i$. L'assegnamento è simultaneo per tutte le variabili.
- **Await $c$**, con $c$ espressione booleana: quando l'espressione diventa $true$ fa avanzare il controllo. Non ha effetto sulle variabili.
- **Send $\alpha \Leftarrow e$**, con $\alpha$ variabile *canale* ed $e$ espressione compatibile con il tipo di $\alpha$. $e$ viene calcolata e assegnata al canale. Questa istruzione è **sempre** attiva se il controllo è di fronte ad essa.
- **Receive $\alpha \Rightarrow u$** assegna la variabile del canale ad $u$. L'istruzione è abilitata se e solo se il canale ha un valore.
- **Request/Release $r$** decresce/aumenta il valore di $r$. **Request** può essere usata solo se $r > 0$.

### Istruzioni schematiche
Rappresentano sequenze di istruzioni per cui solo le condizioni di terminazione sono importanti. Da immaginare come un contratto/interfaccia.
Servono per modellare problemi come la **mutua esclusione** ed il paradigma **producer/consumer**.

- **Noncritical**: un attività di un componente non critica dal punto di vista della gestione delle risorse, poiché vengono utilizzare risorse private. La terminazione non è obbligatoria.
- **Critical**: condensa in un'istruzione unica un'attività critica dal punto di vista della gestione delle risorse e che richiede coordinazione tra i componenti. Deve terminare (almeno giustizia deve essere imposta).
- **Produce $x$**: l'attività assegna alla variabile $x$ un valore diverso da 0. Deve terminare.
- **Consume $y$**: l'attività legge il valore di $y$ senza cambiarlo. Deve terminare.

### Istruzioni composte
- **Condizionale $\text{if} \, c \, \text{then} \, s_{1} \, \text{else} \, s_{2}$** con $c$ condizione booleana, $s_1,s_2$ istruzioni. A differenza di **await**, è sempre attiva ed una delle due istruzioni è sempre eseguita. Un caso particolare è $\text{if} \, c \, \text{then} \, s$: se la condizione risulta falsa l'istruzione termina in uno step.
- **Concatenazione $S_1;S_2;\dots;S_k$**, il primo step dell'esecuzione coincide con il primo step di $S_1$.
- **Selection $S_1$ or $S_2$ or $\dots$ or $S_k$** sceglie non deterministicamente una delle istruzioni abilitate e la esegue; se nessuna istruzione è abilitata l'esecuzione di Selection è posticipata
- **While $c$ do $S$** esegue $S$ finché la condizione $c$ è vera.
- **Cooperation** modella l'esecuzione parallela: all'inizio dell'esecuzione di **cooperation** si sposta il controllo all'inizio di tutte le istruzioni da eseguire, e quando queste sono completate si trasferisce il controllo dalla fine di queste alla fine dell'istruzione **cooperation**.
- **Block** permette di dichiarare localmente delle variabili in base ad una formula/predicato.

### Istruzioni composte raggruppate
Analogamente alle transizioni di un database, le istruzioni composte raggruppate sono usate per rendere alcune istruzioni composte **atomiche**.

**Skip**, **assign**, **await**, **send** e **receive** sono istruzioni composte di base. 
Istruzioni costruite con istruzioni composte sono a loro volta istruzioni composte.

Data $S$ istruzione composta, $\langle S \rangle$ è un'istruzione composta raggruppata se date due istruzioni ed un'istruzione di comunicazione sincrona $C$ $\langle S_{1}; C; S_{2} \rangle$, $S_{1},S_{2}$ non contengono istruzioni di comunicazione sincrona: un'istruzione raggruppata può quindi includere al massimo un'istruzione **send/receive**.
Inoltre, un'istruzione raggruppata non può includere due o più istruzioni di comunicazione asincrona sullo stesso canale.

### Da SPL a FTS

Definiamo un [[Fair Transition System#Definizione di Fair Transition System|FTS]] come:
 - $V = \{y_{1},\dots,y_{n}\} \cup \{\pi\}$, tutte le variabili del programma + una singola variabile di controllo $\pi$, il powerset di tutte le possibili posizioni (potrebbe essere un insieme unico nel caso di un singolo componente).
 - $\theta = \varphi \land \pi$, dove $\varphi$ rappresenta tutte le formule nelle clausole **where** degli assegnamenti alle variabili.
 - $\mathcal{T}$ = transizioni che implementano le istruzioni di SPL + la transizione idle. Tutte le transizioni sono **self-disabling**: cambiano almeno il valore della variabile di controllo.