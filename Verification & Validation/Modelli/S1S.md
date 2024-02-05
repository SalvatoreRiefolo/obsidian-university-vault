## Monadic second order theory of one successor

Il linguaggio $S1S$ su un alfabeto $A$, chiamato $S1S_{A}$, è definito da:
- **Termini** ottenuti da costanti, $0$ e variabili del primo ordine $x,y,z,\dots$ applicando possibilmente la **funzione successore** $+1$
- **Formule atomiche** con le seguenti forme:
	- $t_{1} = t_{2}$
	- $t_{1} < t_{2}$
	- $t_{1} \in X$
	- $t_{1} \in Q_a$ (anche scritta come $Q_a(t_1)$) con $a \in A$ l'insieme di variabili che soddisfa $Q_a$. In una $\omega$-parola $\alpha$ è l'insieme delle posizioni in cui un certo carattere occorre. $X$ è una **variabile del secondo ordine**, cioè un insieme di variabili.
- **Connettivi booleani** e **quantificatori** che connettono le formule atomiche.

Ad esempio, la variabile di secondo ordine quantificata $\forall X$ indica che il predicato che segue deve soddisfare ogni insieme di variabili.

### Interpretazione model-theoretic di $\omega$-parole in $S1S$
Si attribuisce un significato a valori di verità di espressioni e formule di $S1S$.

Per ogni parola $\alpha \in A^{\omega}$ definiamo $\underline \alpha = (\omega, 0, +1, <, \{Q_a\}_{\alpha})$, dove:

- $\omega = \mathbb{N}$
- $\{Q_a\}_{\alpha}$ come $\forall a \in A, \, Q_{a} = \{i \in \omega : \alpha(i) = a\}$, l'insieme degli insiemi di posizioni dei simboli di $\alpha$.

Data una formula $\varphi$ di $S1S_{A}$ il linguaggio definito da $\varphi$ è $L(\varphi) = \{\alpha \in A^{\omega}: \underline \alpha \models \varphi\}$: l'insieme di parole $\alpha$ che rende la formula $\varphi$ vera.

Ad esempio il linguaggio sull'alfabeto $A = \{ a, b, c \}$ definito come "tutte le $\omega$-parole tali che ogni '$a$' è seguita da una '$b$'" può essere rappresentata in $S1S_A$ attraverso la formula $$\forall x (x \in Q_{a} \rightarrow \exists y (x < y \land y \in Q_{b}))$$

---

Si può rimuovere l'indipendenza da $A$ sostituendo $A$ con $\{0,1\}^{n}, \, n = \log_2 |A|$. Si passa in questo modo da $S1S_{A}$ a $S1S$.

Ogni simbolo di una parola infinita $\alpha$ viene sostituito con la sua rappresentazione binaria, e si rimpiazzano i predicati $Q_a, Q_b, \dots$ con un insieme di variabili libere di secondo ordine $X_1,\dots,X_n$. 
Ogni parola $\alpha \in A^\omega$ viene rappresentata come $\underline \alpha = (\omega, 0, +1, <, \{P_k\}_{k=1\dots n})$, con $P_k = \{i \in 0 \dots k : (\alpha(i))_k = 1\}$ per $k = 1 \dots n$.

## Teorema di Büchi

Un $\omega$-linguaggio $L \in A^{\omega}$ è $\omega$-regolare se e solo se è definibile in $S1S$.
Si dimostrano i due versi dell'implicazione.

### $\Rightarrow$

Sia $\mathcal{A} = (\{0,1,2,\dots\}, A, \Delta, 0, F)$ un BA che riconosce $L$.
Sia $\varphi$ una formula di forma $\exists Y_{0} \exists Y_{1} \dots \exists Y_{m} \varphi (Y_{0}Y_{1}\dots Y_{m})$ tale che $\forall \alpha \in A^{\omega}$, $\underline \alpha$ è un modello della formula se e solo se $\alpha$ è accettata da $\mathcal{A}$.
Gli insiemi $Y_{i}$ rappresentano le posizioni dove la computazione di $\mathcal{A}$ è sullo stato $i$. 

La formula $\varphi$ ha 3 congiunti: $\varphi = \varphi_{1} \land \varphi_{2} \land \varphi_{3}$:
- $\varphi_{1}(Y_{1},\dots,Y_{m}) = 0 \in Y_{0} \land \bigwedge\limits_{i \neq j} \nexists y (y \in Y_{i} \land y \in Y_{j})$. Questa formula rappresenta **inizialità** e **unicità dello stato**.
- $\varphi_{2}(Y_{1},\dots,Y_{m}) = \forall x \bigvee\limits_{(i,a,j)\in\Delta}(x \in Y_{i} \land x \in Q_{a} \land x+1 \in Y_j)$. Questa formula rappresenta la **consequenzialità** della computazione.
- $\varphi_{3}(Y_{1},\dots,Y_{m}) = \bigvee\limits_{i \in F} \forall x \exists y : x < y \land y \in Y_{i}$. Questa formula rappresenta **finalità**.

Questa formula è soddisfacibile se $\mathcal{A}$ riconosce $L$.

### $\Leftarrow$
Si sostituisce $S1S$ con una variante ugualmente espressiva: $S1S_{0}$. Le formule di questa variante hanno solo quantificatori su variabili del secondo ordine. Successivamente si mostra che per ogni $\psi \in S1S_0$, $L(\psi)$ è $\omega$-regolare.

Si vuole quindi dimostrare che i due modelli hanno la stessa espressività. Attraverso delle regole di riscrittura, è possibile rappresentare qualunque formula in $S1S$ con una in $S1S_0$.
Ottenuta la formula $S1S_0$, questa può essere tradotta in un BA per induzione sulla struttura di $\psi$.

### Problemi in $S1S$

Come conseguenza del teorema di Büchi, la teoria di $S1S$ è **decidibile** in quanto è possibile convertire una formula in un BA e applicare [[4. Riduzioni#Completezza in $L$ e $NL$#$Reachability$|Reachability]] controllando che uno stato finale venga raggiunto. 

L'emptiness problem è decidibile in tempo polinomiale: si converte da una formula $S1S$ ad un BU e si effettua il controllo.

Il problema di controllare l'apparteneza di una formula alla teoria di $S1S$ è **non-elementary**: non è stato trovato un limite alla torre di esponenziazione che rappresenta la complessità del problema.

## Weak S1S 

Variante di $S1S$ con insiemi finiti.

Un $\omega$-linguaggio $L \subseteq A^{\omega}$ è $\omega$-regolare (definibile in $S1S$) se e solo se è definibile in $wS1S$.

Dal [[Automi su parole infinite#Teorema di McNaughton|teorema di McNaughton]] si ha che $L$ può essere espresso come combinazione booleana di linguaggi di forma $\overrightarrow V$, con $V$ linguaggio regolare.
Si dimostra che $\overrightarrow W$, con $W$ linguaggio regolare, può essere espresso con una formula $wS1S$.

Come enunciato in precedenza, esiste una formula $S1S$ che definisce $W$. È possibile costruire una formula $S1S$ di forma $\psi = (X_1,\dots,X_n,y)$ tale che quando interpretata su parole infinite, vincola i prefissi delle $\omega$-parole fino alla posizione $y$ ad appartenere a $W$.
È sufficiente relativizzare le quantificazioni di secondo ordine a posizioni minori di $y$.

Ad esempio:

- La formula $\varphi = \exists X (0 \in X)$ diventa $\psi = \exists X (\forall x (x \in X \rightarrow x < y) \land 0 \in X)$
- La formula $\varphi = \forall X (0 \in X)$ diventa $\psi = \forall X (\forall x (x \in X \rightarrow x < y) \land 0 \in X)$

Data la formula $S1S$ $\varphi(X_1,\dots,X_n)$ che definisce $W$, la formula $wS1S$ che definisce $\overrightarrow W$ è 

$$\forall x \exists y (x < y \land \psi(X_1,\dots,X_n,y))$$

I quantificatori $\forall x \exists y$ indicano che devono esistere infinitamente tanti prefissi.