## Proprietà di chiusura dei linguaggio regolari

I linguaggio regolari sono **chiusi sotto**:
1. Operazioni booleane $\cup,\cap,\lnot$.
2. Concatenazione $\cdot$.
3. Stella di Kleene $*$.

La chiusura indica che se le operazioni vengono applicate ad uno (o più) linguaggi regolari, il risultato rimane un linguaggio regolare.

### Chiusura sotto complementazione
Sia $L$ un linguaggio regolare; vogliamo dimostrare che $\lnot L$ è un linguaggio regolare.
Definiamo il linguaggio $\overline L$ come $A^{*} - L$, il complemento di $L$.
Dato un [[Automi a stati finiti#Automi a stati finiti deterministici (DFA)|DFA]] $\mathcal{A}$ tale che $L(\mathcal{A}) = L$ definiamo $\mathcal{A}'$, l'automa che accetta $\overline L$, come $\mathcal{A}' = (Q,A,\sigma,q_{0}, Q-F)$. 
Questo automa accetta una stringa nel linguaggio $\overline L$ solo se l'automa originario rifiuta una nel linguaggio $L$.

Le computazioni sono le stesse, cambiano solo le condizioni di accettazione.

### Chiusura sotto intersezione 
#esame

Siano $L_1,L_2$ linguaggi regolari, e $\mathcal{A}_1, \mathcal{A}_2$ automi per $L_1,L_2$.
Definiamo $\mathcal{A}$ per il linguaggio $L_{1}\cap L_{2}$ come $\mathcal{A} = (Q_{1}\times Q_{2}, A, \delta,(q_{0_{1}}, q_{0_{2}}), F_{1}\times F_{2})$.

$\mathcal{A}_1, \mathcal{A}_2$ lavorano in sincronia partendo e finendo nello stesso momento.
La funzione di transizione di $\mathcal{A}$ fa eseguire le funzioni di transizione di $\mathcal{A}_1, \mathcal{A}_2$: $\delta((q_i,q_j),a)= (\delta_1(q_i,a),\delta_2(q_j,a))$.

Essendo FA e RE equivalenti possiamo convertire le RE in FA, calcolare l'intersezione o la complementazione e convertire nuovamente in RE.

### Teorema di Myhill-Nerode

Sia $L \subseteq A^*$. Le seguenti affermazioni sono equivalenti.
1. $L$ è un linguaggio regolare.
2. $L$ può essere espresso come l'unione di classi di relazioni di equivalenza che sono sia right invariant sia di indice finito.
3. $\sim_{L}$ è di indice finito.

$1 \rightarrow 2$
Essendo $L$ regolare esiste un DFA che lo riconosce. 
$L$ può essere espresso come l'unione delle classi di $\sim_{\mathcal{A}}$ tali che includono tutte le stringhe di $x \in A^*$ per cui $\delta(q,x) \in F$ (vengono riconosciute da $\mathcal{A}$).
$\sim_{\mathcal{A}}$ è right invariant e di indice finito da definizione, quindi il punto 2. segue immediatamente.

$2\rightarrow 3$ 
$L$ è l'unione di classi di relazioni $\sim$ che sono right invariant e di indice finito. Tutte queste relazioni sono un **raffinamento** di $\sim_L$: $\forall x,y, x \sim y \rightarrow x \sim_{L}y$.
La relazione $\sim_L$ partiziona $A^*$ in insiemi di stringhe equivalenti che appartengono o non appartengono ad $L$. Le relazioni $\sim$ partizionano queste classi in sottoclassi.
Il numero di classi $\sim$ è un limite superiore al numero di classi $\sim_L$, che sono quindi di indice finito, facendo seguire il punto 3.

$3 \rightarrow 1$
Si costruisce un FA a partire da $\sim_L$.
Sia $\mathcal{A}' = (Q', A, \delta', q_{0}', F')$ un automa dove $Q' = A^{*}\setminus \sim_L$ ($Q'$ è partizionato da $\sim_L$): gli stati sono le classi di $\sim_L$.

Sia $\delta'([x]_{\sim_{L}},a) = [xa]_{\sim_L}$, con $[x]_{\sim_L}$ la classe contenente $x$.
Sia $q_{0'}= [\varepsilon]_{\sim_L}$ la classe di $\varepsilon$.
Sia $F' = \{[x]_{\sim_{L}}: x \in L\}$. Gli stati finali sono le classi per cui le parole appartenenti alla classe appartengono ad $L$.

$\sim_L$ è right invariant: se $y \in [x]_{\sim_{L}} \rightarrow [xa]_{\sim_{L}} = [ya]_{\sim_{L}}$. $\mathcal{A}'$ è quindi **consistente**.
$\mathcal{A}'$ accetta $x$ se e solo se $\delta'(q_0',x)= [x]_{\sim_{L}} \in F$, e da definizione di $F'$ $x \in L$. Il numero di stati di $\mathcal{A}'$ è il numero di classi $\sim_L$.


## Proprietà di chiusura dei linguaggi $\omega$-regolari
#esame

1. Se $V \subseteq A^*$ è regolare, allora $V^{\omega}$ è $\omega$-regolare.

Sia $\mathcal{A}$ un FSA che riconosce $V$. Possiamo assumere senza perdita di generalità che $\varepsilon \notin V$, e che non ci siano archi entranti in $q_0$ nel grafo dell'esecuzione.

Un [[Automi su parole infinite#Automi di Büchi|BA]] per $V^{\omega}$ può essere ottenuto aggiungendo ad $\mathcal{A}$ una transizione $(s, a, q_0)$ (leggendo $a$ nello stato $s$ ci si sposta in $q_0$) per ogni transizione $(s, a, s') \in \Delta, \, s' \in F$ e definendo $q_0$ come unico stato finale del BA. In questo modo dividiamo parole infinite in parole finite appartenenti a $V$.

2. Se $V \subseteq A^*$ è regolare e $L \subseteq A^{\omega}$ è $\omega$-regolare, allora $V \cdot L$ è $\omega$-regolare.

3. Se $L_{1}, L_{2} \subseteq A^{\omega}$ sono $\omega$-regolari, allora $L_{1} \cap L_{2}$ e $L_{1} \cup L_{2}$ sono $\omega$-regolari.

### Chiusura sotto complementazione
Se $L \subseteq A^{\omega}$ è un linguaggio $\omega$-regolare allora $\overline L$ è $\omega$-regolare. È possibile costruire un BA per $\overline L$ partendo da un BA per $L$.

Dato un BA $\mathcal{A}$ ed una [[Relazioni#Relazione $ approx_{ mathcal{A}}$|congruenza di indice finito]] $\approx_{\mathcal{A}}$ possiamo derivare dai due lemmi sulla relazione che  $\overline L = \bigcup U \cdot V^{\omega}$ per $U,V$ $\approx_{\mathcal{A}}$-classi  tali che $U \cdot V^{\omega} \cap \overline L \neq \varnothing$. Essendo $U,V$ linguaggi regolari ed i $\omega$-linguaggi chiusi sotto concatenazione e unione, un BA che riconosce questi linguaggi riconosce anche $\overline L$.

Una conseguenza della chiusura sotto complementazione è che due gli $\omega$-linguaggi sono univocamente identificati dalle lor $\omega$-parole $UP$.

#todo dimostrazione