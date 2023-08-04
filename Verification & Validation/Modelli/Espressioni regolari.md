
## Espressioni regolari ristrette

Sono costruite a partire da un insieme finito di parole su un alfabeto $A$, che assumiamo essere $A$ stesso. Ad esempio, se $A=\{a,b,c\}$ l'insieme di parole è $\{a,b,c\}$.

Si costruiscono applicando sull'insieme le operazioni di unione, concatenazione e stella di Kleene (*).

Esempi:
- $r \cup s$ è l'unione delle espressioni regolari $r,s$, cioè l'unione degli insiemi di stringhe da esse identificate.
- $r \cdot s$ è la concatenazione delle espressioni regolari.
- $r* = \{\varepsilon, a,b,c,aa,ab,ac,ba,bb,\dots,aaa,\dots\}$.

## Espressioni regolari generali

Costruite da un alfabeto $A$ applicando operazioni di unione, concatenazione, stella di Kleene e negazione ($\lnot$) che rappresenta la complementazione rispetto ad $A^*$.
Hanno lo stesso potere espressivo delle espressioni regolari ristrette.

## Espressioni regolari star-free

Sono espressioni regolari generali senza la stella di Kleene.

## Teorema: equivalenza tra espressioni regolari e FA

Per ogni automa finito esiste un'espressione regolare ristretta che definisce lo stesso linguaggio e viceversa.

Dimostriamo i due versi dell'implicazione.

### FA $\Rightarrow$ RRE
#todo 
Dato $\mathcal{A} = (\{q_1,\dots,q_n\},A,\delta,q_1,F)$ un automa a stati finiti definiamo $R_{i,j}^{k}$ come l'insieme di parole $x$ tali che $\delta(q_{i,x})= q_j$ e per ogni $y$ prefisso di $x$, con $y \neq x,y\neq \varepsilon$ si ha che $\delta(q_{i,y})= q_{l}, l\leq k$.
Applicando la funzione di transizione sulla parola $x$ in stato $q_i$ raggiungiamo $q_j$), ed applicando la funzione di transizione sulla parola $y$ con lo stesso stato raggiungiamo lo stato $q_l$ con indice $\leq k$.

### RRE $\Rightarrow$ NDA con $\varepsilon$-mosse
#todo 

## Teorema: chiusura dei linguaggio regolari

I linguaggio regolari sono **chiusi sotto**:
1. Operazioni booleane $\cup,\cap,\lnot$.
2. Concatenazione $\cdot$.
3. Stella di Kleene $*$.

La chiusura indica che se le operazioni vengono applicate ad una (o più) espressioni regolari, il risultato rimane un'espressione regolare.

### Chiusura sotto complementazione
Sia $L$ un linguaggio regolare; vogliamo dimostrare che $\lnot L$ è un linguaggio regolare.
Definiamo il linguaggio $\overline L$ come $A^{*} - L$, il complemento di $L$.
Dato un DFA $\mathcal{A}$ tale che $L(\mathcal{A}) = L$ definiamo $\mathcal{A}'$, l'automa che accetta $\overline L$, come $\mathcal{A}' = (Q,A,\sigma,q_{0}, Q-F)$. 
Questo automa accetta una stringa nel linguaggio $\overline L$ solo se l'automa originario rifiuta una nel linguaggio $L$.

Le computazioni sono le stesse, cambiano solo le condizioni di accettazione.

### Chiusura sotto intersezione 
#esame

Siano $L_1,L_2$ espressioni regolari, e $\mathcal{A}_1, \mathcal{A}_2$ automi per $L_1,L_2$.
Definiamo $\mathcal{A}$ per il linguaggio $L_{1}\cap L_{2}$ come $\mathcal{A} = (Q_{1}\times Q_{2}, A, \delta,(q_{0_{1}}, q_{0_{2}}), F_{1}\times F_{2})$.

$\mathcal{A}_1, \mathcal{A}_2$ lavorano in sincronia partendo e finendo nello stesso momento.
La funzione di transizione di $\mathcal{A}$ fa eseguire le funzioni di transizione di $\mathcal{A}_1, \mathcal{A}_2$: $\delta((q_i,q_j),a)= (\delta_1(q_i,a),\delta_2(q_j,a))$.

Essendo FA e e RE equivalenti possiamo convertire le RE in FA, calcolare l'intersezione o la complementazione e convertire nuovamente in RE.

## Espressioni $\omega$-regolari