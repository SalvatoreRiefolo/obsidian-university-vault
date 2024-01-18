Le espressioni regolari sono un modo per definire i **linguaggi regolari**, una classe di linguaggi formali, rispetto ad un alfabeto $A$.

## Espressioni regolari ristrette

Sono costruite a partire da insiemi finiti di parole su un alfabeto $A$, che assumiamo essere $A$ stesso. Ad esempio, se $A=\{a,b,c\}$ l'insieme di parole è $\{a,b,c\}$.

Si costruiscono applicando sugli insiemi le operazioni di unione, concatenazione e stella di Kleene (*).

Esempi:
- $r \cup s$ è l'unione delle espressioni regolari $r,s$, cioè l'unione degli insiemi di stringhe da esse identificate.
- $r \cdot s$ è la concatenazione delle espressioni regolari.
- $r* = \{\varepsilon, a,b,c,aa,ab,ac,ba,bb,\dots,aaa,\dots\}$.

## Espressioni regolari generali

Costruite da un alfabeto $A$ applicando operazioni di unione, concatenazione, stella di Kleene e negazione ($\lnot$) che rappresenta la complementazione rispetto ad $A^*$.
Hanno lo stesso potere espressivo delle espressioni regolari ristrette.

## Espressioni regolari star-free

Sono espressioni regolari generali senza la stella di Kleene.

## Teorema: equivalenza tra espressioni regolari e FSA

Per ogni automa finito esiste un'espressione regolare ristretta che definisce lo stesso linguaggio e viceversa.

Si dimostrano i due versi dell'implicazione.

### FSA $\Rightarrow$ RRE
#todo 
Dato $\mathcal{A} = (\{q_1,\dots,q_n\},A,\delta,q_1,F)$ un automa a stati finiti definiamo $R_{i,j}^{k}$ come l'insieme di parole $x$ tali che $\delta(q_{i},x)= q_j$ e per ogni $y$ prefisso di $x$, con $y \neq x,y\neq \varepsilon$ si ha che $\delta(q_{i},y)= q_{l}, l\leq k$.

Applicando la funzione di transizione sulla parola $x$ in stato $q_i$ raggiungiamo $q_j$, ed applicando la funzione di transizione sulla parola $y$ con lo stesso stato raggiungiamo lo stato $q_l$ con indice $\leq k$.

### RRE $\Rightarrow$ NDA con $\varepsilon$-mosse
#todo 


## Espressioni $\omega$-regolari

Sono definite come $\bigcup\limits_{i=1}^{n} u_{i}\cdot V_{i}^{\omega}$, con $u_{i}, V_{i}$ linguaggi regolari.

## Relazione tra linguaggi $\omega$-regolari ed espressioni $\omega$-regolari

Si definisce con $s \rightarrow_{w} s'$ l'esistenza di una computazione di un automa $\mathcal{A}$ su parola $w$ che muove lo stato da $s$ a $s'$. 
Per ogni coppia di stati $s,s'$ definiamo l'insieme $W_{s,s'} = \{w: s \rightarrow_{w} s'\}$; questo insieme è un linguaggio regolare.
Possiamo quindi definire un FSA $\mathcal{A}$ che riconosce questo linguaggio come $(Q,A,\Delta,s,{s'})$.

## Teorema: equivalenza tra espressioni $\omega$-regolari e BA
Per qualunque BA esiste un'espressione $\omega$-regolare che definisce lo stesso linguaggio e viceversa.

Si dimostrano i due versi dell'implicazione.

### $\omega$-RE $\Rightarrow$ BA
Da definizione $L = \bigcup\limits_{i=1}^{n} u_{i}\cdot V_{i}^{\omega}$. 
- $u_i$ può essere deciso da un FSA.
- $V_{i}^{\omega}$ è $\omega$-regolare, e può essere riconosciuto da un BA.

Per le proprietà di chiusa dei BA, la concatenazione crea un linguaggio $\omega$-regolare riconoscibile da un BA.

### BA $\Rightarrow$ $\omega$-RE
Un BA raggiunge nel suo grafo di esecuzione uno stato finale infinite volte, partendo dallo stato di partenza $q_0$. Esiste quindi un percorso infinito $W_{q_{0}, s} \cdot W^{\omega}_{s,s}$, con $s$ uno dei possibili stati finali. La definizione finale è dunque $\bigcup\limits_{s \in F} W_{q_{0}, s} \cdot W^{\omega}_{s,s}$.


## $\omega$-parole ultimately periodic

Una $\omega$-parola $\alpha \in A^{\omega}$ è **definitivamente periodica** (ultimately periodic, $UP$) se $\alpha = u \cdot v^{\omega}$, con $u,v \in A^*$. $u$ è il *prefisso finito*, mentre $v$ è il *periodo*.

Segue come corollario che ogni linguaggio $\omega$-regolare non vuoto include una parola $UP$.
Per dimostrarlo prendiamo un linguaggio $L=L(\mathcal{A})$ per qualche $\mathcal{A}$. Dal [[Espressioni regolari#Teorema equivalenza tra espressioni $ omega$-regolari e BA|teorema precedente]], $L = \bigcup\limits_{i=1}^{n} u_i \cdot v_i^{\omega}$. Dato che $L$ non è vuoto esiste una $\omega$-parola $\alpha \in u_i \cdot v_i^{\omega}$ per qualche indice $i$.
Segue che la parola $\alpha' = u \cdot v_{1} \cdot v_{1} \cdot \dots \in L$ è riconosciuta.