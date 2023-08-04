
Sia $A$ un insieme generico chiuso sotto concatenazione.

## Right invariant equivalence relation

Una **relazione di equivalenza** $\sim$ su $A$ è **right invariant** in relazione alla concatenazione se: 
$$\forall x,y \in A, x \sim y \rightarrow \forall z \in A, xz \sim yz$$

La concatenazione **a destra** preserva l'equivalenza.

---

Sia $L \subseteq A^*$ un linguaggio.

La relazione $\sim_L$ su $A^*$ è definita come:

$$\forall x,y \in A^{*}, x \sim_L y \rightarrow \forall z \in A^{*}, xz \in L \leftrightarrow yz \in L$$

$x,z$ sono in relazione rispetto ad un linguaggio $L$ se concatenando una stringa $z$ ad $x,y$ entrambe le stringhe risultanti appartengono (o non appartengono) ad $L$ .

Questa relazione è di equivalenza ed è right invariant.
#todo

---

Sia $\mathcal{A}$ un [[Automi a stati finiti#Automi a stati finiti deterministici (DFA)|DFA]].
La relazione $\sim_{\mathcal{A}}$ su $A^*$ è definita come:

$$\forall x,y \in A^{*}, x \sim_{\mathcal{A}} y \leftrightarrow \delta(q_{0},x) = \delta(q_{0},y)$$

$x,z$ sono in relazione rispetto ad un DFA $\mathcal{A}$ se lo stato raggiunto dallo stato di input $q_0$ su entrambe le stringhe è lo stesso.

Questa relazione è di equivalenza ed è right invariant.
#todo

La cardinalità di questa relazione è limitata dal numero di stati $|Q|$ di $\mathcal{A}$: due stringhe sono in relazione si raggiunge lo stesso stato da quello iniziale, ed abbiamo un numero finito di stati, $|Q|$. La relazione ha quindi **indice finito**.

## Teorema di Myhill-Nerode

Sia $L \subseteq A^*$. Le seguenti affermazioni sono equivalenti.
1. $L$ è un linguaggio regolare.
2. $L$ può essere espresso come l'unione delle classi di relazioni di equivalenza che sono sia right invariant sia di indice finito.
3. $\sim_{L}$ è di indice finito.

### 1. $\rightarrow$ 2.
Essendo $L$ regolare esiste un DFA che lo riconosce. 
$L$ può essere espresso come l'unione delle classi di $\sim_{\mathcal{A}}$ tali che includono tutte le stringhe di $x \in A^*$ per cui $\delta(q,x) \in F$ (vengono riconosciute da $\mathcal{A}$).
$\sim_{\mathcal{A}}$ è right invariant e di indice finito da definizione, quindi il punto 2. segue immediatamente.

### 2. $\rightarrow$ 3.
$L$ è l'unione di classi di relazioni $\sim$ che sono right invariant e di indice finito. Tutte queste relazioni sono un **raffinamento** di $\sim_L$: $\forall x,y, x \sim y \rightarrow x \sim_{L}y$.
La relazione $\sim_L$ partiziona $A^*$ in insiemi di stringhe equivalenti che appartengono o non appartengono ad $L$. Le relazioni $\sim$ partizionano queste classi in sottoclassi.
Il numero di classi $\sim$ è un limite superiore al numero di classi $\sim_L$, che sono quindi di indice finito, facendo seguire il punto 3.

### 3. $\rightarrow$ 1.
Si costruisce un FA a partire da $\sim_L$.
Sia $\mathcal{A}' = (Q', A, \delta', q_{0}', F')$ un automa dove $Q' = A^{*}\setminus \sim_L$ ($Q'$ è partizionato da $\sim_L$): gli stati sono le classi di $\sim_L$.

Sia $\delta'([x]_{\sim_{L}},a) = [xa]_{\sim_L}$, con $[x]_{\sim_L}$ la classe contenente $x$.
Sia $q_{0'}= [\varepsilon]_{\sim_L}$ la classe di $\varepsilon$.
Sia $F' = \{[x]_{\sim_{L}}: x \in L\}$. Gli stati finali sono le classi per cui le parole appartenenti alla classe appartengono ad $L$.

$\sim_L$ è right invariant: se $y \in [x]_{\sim_{L}} \rightarrow [xa]_{\sim_{L}} = [ya]_{\sim_{L}}$. $\mathcal{A}'$ è quindi **consistente**.
$\mathcal{A}'$ accetta $x$ se e solo se $\delta'(q_0',x)= [x]_{\sim_{L}} \in F$, e da definizione di $F'$ $x \in L$. Il numero di stati di $\mathcal{A}'$ è il numero di classi $\sim_L$.


## Congruenza e saturazione