Si passa da parole finite a parole **infinite** su un insieme **finito** $A$; queste parole vengono chiamate **$\omega$-parole**.
Si passa da $A^*$ ad $A^{\omega}$, l'universo di tutte le parole infinite costruibili su $A$. Si definisce $A^{\infty} = A^{*} \cup A^{\omega}$.
Si usano $\alpha, \beta, \gamma,\dots$ per denotare le $\omega$-parole. Le $\omega$-parole sono composte da un infinita successione di simboli $\alpha(1)\alpha(2)\dots$, con $\alpha(i) \in A$. 
Dati due indici $n,m$, $a(n,m) = \alpha(n)\alpha(n+1),\dots,\alpha(m-1)$ è la **sotto parola finita** tra gli indici $n,m$ con $n \leq m$.
Si definisce $\exists^{\omega}_n$ come "esistono infinitamente tante posizioni $n$" e $\exists^{< \omega}_n$ con "esistono un numero finito di $n$".

Dato un insieme $W \subseteq A^*$ (insieme infinito di parole finite) si definiscono i seguenti insiemi di parole infinite:
- $W^{\omega} = \{\alpha \in A^{\omega}: \alpha = w_{0}w_{1}\dots w_{i} \in W, \, \forall i \geq 0\}$, chiamato **$\omega$-chiusura** di $W$.
- $\overrightarrow{W} = \{\alpha \in A^{\omega}: \exists^{\omega}_n \alpha(0, n) \in W\}$, chiamato **chiusura vettoriale** di $W$. È l'insieme infinito di parole con prefissi appartenenti a $W$.

$W^{\omega}, \overrightarrow{W}$ possono coincidere (ad esempio quando $W$ ha forma $(ab)^*$) o essere diversi (ad esempio quando $W$ ha forma $(a^*b) \cup (cd^*)$)

Si definiscono con $In(\alpha) = \{a \in A: \exists^{\omega}_{n}\alpha(n) = a\}$ i simboli di $A$ che occorrono infinite volte in $\alpha$.

## Automi di Büchi

Un **BA** $\mathcal{A}$ è una quintupla $(Q,A,\Delta, q_{0}, F)$. La definizione è la stessa del [[Automi a stati finiti#Automi a stati finiti non deterministici (NFA)|NFA]] ($\Delta$ è una relazione di transizione non deterministica).

Una computazione $\delta$ di $\mathcal{A}$ su una $\omega$-parola è una $\omega$-parola su $Q$ tale che:
1. $\delta(0) = q_0$, il primo simbolo della $\omega$-parola $\delta$ è lo stato $q_0$.
2. $\forall i \geq 0, \, (\delta(i), \alpha(i)\delta(i+1)) \in \Delta$, lo stato raggiunto dallo stato $\delta(i)$ leggendo il simbolo $\alpha(i)$ è nella relazione di transizione.

Una computazione $\delta$ su $\alpha$ ha successo se $In(\alpha) \cap F \neq \varnothing$: alcuni stati finali in $F$ sono raggiunti infinite volte.

$\mathcal{A}$ **accetta** una $\omega$-parola $\alpha$ se e solo se esiste una computazione di successo di $\mathcal{A}$ su $\alpha$. Il **linguaggio** $L(\mathcal{A})$ è l'insieme di tutte le $\omega$-parole accettate da $\mathcal{A}$.

Un linguaggio $L \subseteq A^{\omega}$ è **$\omega$-regolare** se esiste un BA $\mathcal{A}$ tale che $L = L(\mathcal{A})$

## Proprietà di chiusura dei linguaggi $\omega$-regolari
#esame

1. Se $V \subseteq A^*$ è regolare, allora $V^{\omega}$ è $\omega$-regolare.

Sia $\mathcal{A}$ un FSA che riconosce $V$. Possiamo assumere senza perdita di generalità che $\varepsilon \notin V$, e che non ci siano archi entranti in $q_0$ nel grafo dell'esecuzione.

Un BA per $V^{\omega}$ può essere ottenuto aggiungendo ad $\mathcal{A}$ una transizione $(s, a, q_0)$ (leggendo $a$ nello stato $s$ ci si sposta in $q_0$) per ogni transizione $(s, a, s') \in \Delta, \, s' \in F$ e definendo $q_0$ come unico stato finale del BA. In questo modo dividiamo parole infinite in parole finite appartenenti a $V$.

2. Se $V \subseteq A^*$ è regolare e $L \subseteq A^{\omega}$ è $\omega$-regolare, allora $V \cdot L$ è $\omega$-regolare.

3. Se $L_{1}, L_{2} \subseteq A^{\omega}$ sono $\omega$-regolari, allora $L_{1} \cap L_{2}$ e $L_{1} \cup L_{2}$ sono $\omega$-regolari.

### Chiusura sotto complementazione
Se $L \subseteq A^{\omega}$ è un linguaggio $\omega$-regolare allora $\overline L$ è $\omega$-regolare. È possibile costruire un BA per $\overline L$ partendo da un BA per $L$.

Dato un BA $\mathcal{A}$ ed una [[Relazioni#Relazione $ approx_{ mathcal{A}}$|congruenza di indice finito]] $\approx_{\mathcal{A}}$ possiamo derivare dai due lemmi sulla relazione che  $\overline L = \bigcup U \cdot V^{\omega}$ per $U,V$ $\approx_{\mathcal{A}}$-classi  tali che $U \cdot V^{\omega} \cap \overline L \neq \varnothing$. Essendo $U,V$ linguaggi regolari ed i $\omega$-linguaggi chiusi sotto concatenazione e unione, un BA che riconosce questi linguaggi riconosce anche $\overline L$.

Una conseguenza della chiusura sotto complementazione è che due gli $\omega$-linguaggi sono univocamente identificati dalle lor $\omega$-parole $UP$.

#todo dimostrazione

## Automi di Büchi deterministici

Un **DBA** $\mathcal{A}$ è una quintupla $(Q,A,\delta, q_{0}, F)$. La definizione è la stessa dell'automa di Büchi non deterministico, ma $\delta$ è una funzione di transizione deterministica $Q \times A \rightarrow Q$

Una computazione $\sigma$ su $\alpha$ ha successo se $In(\alpha) \cap F \neq \varnothing$: alcuni stati finali in $F$ sono raggiunti infinite volte.

I DBA sono chiusi sotto unione ed intersezione.

Un linguaggio $L \in A^{\omega}$ è riconosciuto da un DBA se e solo se $L = W$ per qualche linguaggio regolare $W \subseteq A^*$. Si dimostrano i due versi dell'implicazione.

$\Rightarrow$) Sia $\mathcal{A}$ un DBA, $\mathcal{A}'$ il FSA su parole finite corrispondente e $W$ il linguaggio riconosciuto da $\mathcal{A}'$. Si dimostra che $L(\mathcal{A}) = \overrightarrow W$, l'insieme infinito di prefissi.
Dalla definizione di condizione di accettazione dei BA e dalla loro natura deterministica, $\mathcal{A}$ accetta una $\omega$-parola $\alpha$ se e solo se la computazione di $\mathcal{A}$ su $\alpha$ ha successo: ad infinite posizioni si trovano infiniti stati finali. 
Si possono trovare infiniti prefissi $\in W$ nell'esecuzione di $\mathcal{A}'$, validando la condizione di accettazione.

$\Leftarrow$ Sia $\alpha \in \overrightarrow W$, con $W$ linguaggio regolare.
Dato che $W$ è regolare esiste un DFA $\mathcal{A}'$ tale che $W = L(\mathcal{A}')$. La computazione di successo di $\mathcal{A}'$ su un qualunque prefisso può essere vista come il prefisso della computazione *unica* $\delta$ del corrispondente DBA $\mathcal{A}$.
Deduciamo che $\alpha \in L(\mathcal{A})$ perché $\delta$ passa attraverso uno stato finale infinite volte.

I DBA **non** son chiusi sotto complementazione: DBA $\subsetneq$ NBA.

## Automi di Muller

Un **DMA** $\mathcal{A}$ è una quintupla $(Q,A,\delta, q_{0}, \mathcal{F})$. 
$\mathcal{F}$ è un sottoinsieme del power set degli stati $2^Q$, ed è l'insieme di insiemi di stati finali. 
Una computazione $\sigma$ di $\mathcal{A}$ su una $\omega$-parola $\alpha$ ha successo se e solo se ad infinite posizioni si trova un sottoinsieme di stati $\in \mathcal{F}$.

La versione non determinista, chiamata **NMA**, si ottiene sostituendo alla funzione $\delta$ la relazione $\Delta$.

### Relazione tra DMA e DBA
È semplice dimostrare che ogni DBA è equivalente ad un DMA: sia $\mathcal{A}$ un DBA. Il DMA $\mathcal{A}'$ equivalente si ottiene definendo come $\mathcal{F} = \{P \subset Q : P \cap F \neq \varnothing \}$, l'insieme di stati che sono finali in $\mathcal{A}$.

Segue che:
 - NBA $\equiv$ NMA
 - NMA $\equiv$ DMA
 - DBA $\subsetneq$ DMA
 - DBA $\subsetneq$ NBA
 - NBA $\equiv$ DMA

NBA e NMA hanno lo stesso potere espressivo, ed è possibile ottenere l'uno dall'altro passando per una formula $S1S$.

### Proprietà di chiusura
I DMA sono chiusi sotto unione, intersezione e complementazione.

### Lemmi
Un $\omega$-linguaggio $L \subseteq A^{\omega}$ è riconosciuto da un DMA se e solo se $L$ una **combinazione booleana** su $A^{\omega}$ di linguaggio della forma $\overrightarrow W$, dove $W$ è un insieme regolare.

Ogni $\omega$-linguaggio $L \subseteq A^{\omega}$ riconosciuto da un NBA può essere espresso come l'unione finita di linguaggio della forma $\overrightarrow U \cap \overline{\overrightarrow V}$ con $U,V$ linguaggio regolari.


#todo dimostrazione 74

## Teorema di McNaughton

Un $\omega$-linguaggio è $\omega$-regolare se e solo se è riconosciuto da un DMA. Segue che NBA $\equiv$ DMA. 
Si dimostrano i due versi dell'implicazione.

$\Rightarrow$: segue dai due lemmi precedenti.
$\Leftarrow$: dal primo lemma, un linguaggio riconosciuto da un DMA può essere espresso come la combinazione booleana dei linguaggio di forma $\overrightarrow U$, con $U$ linguaggio regolare. $\overrightarrow U$ può essere riconosciuto da un DBA, e per le proprietà di chiusura dei DBA può essere riconosciuto anche da un NBA.
