
Sia $A$ un insieme generico chiuso sotto concatenazione.

## Right invariant equivalence relation

Una **relazione di equivalenza** $\sim$ su un insieme $A$ è **right invariant** rispetto alla concatenazione se: 
$$\forall x,y \in A, x \sim y \rightarrow \forall z \in A, xz \sim yz$$

La concatenazione **a destra** preserva l'equivalenza.

---

Sia $L \subseteq A^*$ un linguaggio.

La relazione $\sim_L$ su $A^*$ è definita come:

$$\forall x,y \in A^{*}, x \sim_L y \rightarrow \forall z \in A^{*}, xz \in L \leftrightarrow yz \in L$$

$x,z$ sono in relazione rispetto ad un linguaggio $L$ se concatenando una stringa $z$ ad $x,y$ entrambe le stringhe risultanti appartengono (o non appartengono) ad $L$ .

Questa relazione è di equivalenza ed è right invariant.

---

Sia $\mathcal{A}$ un [[Automi a stati finiti#Automi a stati finiti deterministici (DFA)|DFA]].
La relazione $\sim_{\mathcal{A}}$ su $A^*$ è definita come:

$$\forall x,y \in A^{*}, x \sim_{\mathcal{A}} y \leftrightarrow \delta(q_{0},x) = \delta(q_{0},y)$$

$x,y$ sono in relazione rispetto ad un DFA $\mathcal{A}$ se lo stato raggiunto dallo stato di input $q_0$ su entrambe le stringhe è lo stesso.

Questa relazione è di equivalenza ed è right invariant.

La cardinalità di questa relazione è limitata dal numero di stati $|Q|$ di $\mathcal{A}$: due stringhe sono in relazione se si raggiunge lo stesso stato da quello iniziale, ed abbiamo un numero finito di stati $|Q|$. La relazione ha quindi **indice finito**.

## Congruenza

Una relazione di equivalenza $\sim$ su un insieme $A$ è una **congruenza** in relazione alla concatenazione se:

$$\forall x,x',y,y', \, x \sim y \land x' \sim y' \rightarrow x \cdot x' \sim y \cdot y'$$

Ad esempio date due parole $x,y \in A^*$ tali che $x \sim y$, se la parità del numero di caratteri di $x$ è uguale alla parità del numero di caratteri di $y$ allora queste appartengono alla stessa classe di equivalenza, e la relazione è una congruenza. In particolare, questa relazione divide $A^*$ in due sottoinsiemi, quella delle parole di lunghezza pari e quella di lunghezze dispari.

## Saturazione

Una congruenza $\sim$ su $A^*$ **satura** un $\omega$-linguaggio $L \subseteq A^{\omega}$ se per ogni coppia di $\sim$-classi:

$$U \cdot V^{\omega} \cap L \neq \varnothing \rightarrow U \cdot V^{\omega} \subseteq L$$

Se c'è almeno una $\omega$-parola nel linguaggio allora la concatenazione di parole nelle due $\sim$-classi è un sottoinsieme del linguaggio.

Se $\sim$ è una congruenza e $L \subset A^{\omega}$ è $\omega$-regolare, se $\sim$ satura $L$ allora satura anche $\overline L = A^{\omega} - L$.
Si dimostra per contraddizione, affermando che $\sim$ satura $L$ ma non $\overline L$, e quindi affermando di conseguenza che esistono due classi di $\sim$ $U,V$ tali che almeno una parola è in $\overline L$ ma $U \cdot V^{\omega} \nsubseteq \overline L$.
Da questa affermazione segue che esiste una $\omega$-parola non in $\overline L$ che deve quindi essere in $L$, ma dato che $\sim$ satura $L$ allora $U \cdot V^{\omega} \subseteq L$, raggiungendo così una contraddizione.

## Relazione $\approx_{\mathcal{A}}$
Sia $\mathcal{A}$ un BA. Le parole $u,v$ appartengono alla relazione $u \approx_{\mathcal{A}} v$ su $A^*$ se $\forall s,s' \in Q$:

- $s \rightarrow_{v} s' \leftrightarrow s \rightarrow_{u} s'$
- $s \rightarrow_{v}^{F} s' \leftrightarrow s \rightarrow_{u}^{F} s'$

$u,v$ si comportano allo stesso modo, ma i percorsi sul grafo computazionale potrebbero essere differenti.

**Lemma 1**: la relazione $\approx_{\mathcal{A}}$ ha le seguenti proprietà:
1. È una congruenza.
2. Ha indice finito.
3. Satura $L(\mathcal{A})$. 

Per dimostrare 3., si consideri un BA $\mathcal{A}$ e $u,v \approx_{\mathcal{A}}$-classi. 
Si consideri $\alpha \in U \cdot V^{\omega} \cap L(\mathcal{A}) \neq \varnothing$: $\alpha \in L(\mathcal{A})$. Esiste quindi una computazione di successo di $\mathcal{A}$ su $\alpha$, cioè uno stato finale è raggiunto infinite volte. Dato che $\alpha \in U \cdot V^{\omega}$ esistono $u \in U, v_1,v_2,\dots, \in V$ tali che $\alpha = u\cdot v_{1}\cdot v_{2}\cdot\dots$. 
Da una sequenza di successo possiamo ottenere una sequenza di step $q_{0}, s_{1}, s_{2},\dots$ tali che $q_0 \rightarrow_{u} s_{1}, s_1 \rightarrow_{v_1} s_{2}, \dots$ e per un numero infinito di $i$ vale che $s_{i} \rightarrow_{v}^{F} s_{i+1}$. 
Prendiamo una successione candidata $\mathcal{B} = u'v_{1}'v_{2}'\dots \in U \cdot V^{\omega}$ e dimostriamo che $\mathcal{B} \in L(\mathcal{A})$. Dato che $u \approx_{\mathcal{A}} u'$, il primo step della sequenza è lo stesso, e dopo di che abbiamo una sequenza di stati che passa infinite volte per uno stato finale. Concludiamo quindi che $\mathcal{B} \in L(\mathcal{A})$.

**Lemma 2**: sia $\sim$ una congruenza di indice finito su $A^*$. Per ogni $\omega$-parola $\alpha \in A^{\omega}$ esistono due classi $U,V$ di $\sim$ tali che $\alpha \in U \cdot V^{\omega}$, con $V \cdot V \subseteq V$.
La dimostrazione (omessa) sfrutta il teorema di Ramsey.

Dai due lemmi segue che: dato un $\omega$-linguaggio $L \subseteq A^{\omega}$ ed una congruenza $\sim$ di indice finito che satura $L$, $L = \bigcup U \cdot V^{\omega}$, con $U,V$ $\sim$-classi tali che $U \cdot V^{\omega} \cap L \neq \varnothing$.
Unendo tutte le classi di congruenza che saturano un linguaggio otteniamo il linguaggio stesso.