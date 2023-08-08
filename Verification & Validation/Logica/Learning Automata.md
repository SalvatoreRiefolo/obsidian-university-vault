L'apprendimento viene interpretato come un **gioco**: 
1. Il **teacher** sceglie un linguaggio **regolare** (FSA, RE) $L_o$.
2. Il **learner** conosce l'alfabeto $A$ del linguaggio e può porre due tipi di domanda al teacher:
	- **Appartenenza** di una parola al linguaggio: $w \in L_{o}?$
	- **Equivalenza** tra un linguaggio ed $L_o$: $L_{o} = L(\mathcal{A})?$. $\mathcal{A}$ è l'automa che il learner ha prodotto.
3. Il teacher risponde:
	- Se $w \in L_o$ risponde sì, altrimenti no.
	- Se $L_{o} = L(\mathcal{A})$ risponde sì, ed il learner vince, altrimenti il teacher da il più breve controesempio.

Il processo si ripete dallo step .2 finché il learner vince. È possibile che il learner vinca solamente con i due controlli sopracitati, generando l'automa e controllando fino alla vittoria. Tuttavia usando una **strategia**, è possibile raggiungere la vittoria in tempo polinomiale rispetto alla dimensione dell'automa che decide $L_0$: $\Theta(|\mathcal{A}_o|)$.

## Applicazione dell'equivalenza di Myhill-Nerode

Si sfrutta l'equivalenza di [[Relazioni#Teorema di Myhill-Nerode|Myhill-Nerode]] (MN da qua in avanti), definita come:

$$u \sim_{L_{o}} v = 
\forall t \in \Sigma^{*}, u \cdot t \in L_{o} \leftrightarrow v \cdot t \in L_{o}$$

La doppia implicazione $\leftrightarrow$ e l'appartenenza a $L_o$ possono essere riscritte come $=_{L_0}$: $u \cdot t =_{L_{0}} v \cdot t$.
La relazione ha indice finito se il linguaggio è regolare.

Si definisce anche l'equivalenza rispetto ad un insieme finito di parole di test $T$:

$$u \approx_{L_{o}T} v = 
\forall t \in T, u \cdot t \in L_{o} \leftrightarrow v \cdot t \in L_{o}$$

Questa equivalenza è meno generale e ha meno classi rispetto a MN, e **non satura** $L$: potrebbe contenere alcune parole non presenti nel linguaggio.

---

Un insieme $S \subseteq \Sigma^*$ è **$T$-minimale** se $\forall s \neq s' \in S, s \not\approx_{L_{o}T} s'$: non è possibile trovare due stringhe di $S$ nella stessa classe.

Un insieme $S \subseteq \Sigma^*$ è **$T$-completo** se $\forall s\in S, \forall a \in \Sigma, \exists s' \in S \, s' \approx_{L_{o}T} s \cdot a$: se si concatena un simbolo ad una stringa in $S$, potrebbe essere nella stessa classe di un'altra stringa di $S$.

#todo pagina 65-68