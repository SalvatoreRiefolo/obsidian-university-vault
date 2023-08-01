## Sintassi
La sintassi degli FA (Finite state Automata) è composta da:
- Un **alfabeto** $A = \{a,b,c,\dots\}$, insieme di simboli.
- **Parole** $w$ su $A$, sequenze finite di simboli di $A$. Ad esempio $A = \{a,b\}, w = aaba$.
- La **lunghezza** di $w$ è il numero di simboli da cui è composta. La parola **vuota** $\varepsilon$ ha lunghezza 0.
- L'operazione fondamentale di **concatenazione** di coppie di parole. Date $u,v$ parole su $A$, la loro concatenazione $u\cdot v$ è la loro giustapposizione $uv$. Ad esempio $A = \{a,b\}, u = aab,v = bba, u\cdot v = aabbba$. L'operazione non è commutativa.
	- Dati $A,B$ alfabeti, $A \cdot B = \{uv : u \in A, v \in B\}$ .
	- $A^n$ insieme di parole di $n$ simboli.
	- $A^*$ insieme di tutte le parole finite composte da simboli di $A$.
	- $A^{+}= A^{*} - \{\varepsilon\}$ 

Ogni insieme di parole su $A$ è un sottoinsieme di $A^*$ ed è chiamato **linguaggio** $L \subseteq A^*$.

## Automi a stati finiti deterministici (DFA)

Un DFA $\mathcal{A}$ è una tupla $(Q,A,\delta, q_{0}, F)$ dove:
- $Q$ è un insieme finito di stati.
- $A$ è un alfabeto finito.
- $\delta: Q \times A \rightarrow Q$  è una funzione di transizione che dato uno stato ed un simbolo di $A$ restituisce un nuovo stato. La funzione è deterministica e potrebbe essere *parziale* (non definita per certi stati).
- $q_{0}$ è lo stato iniziale.
- $F \subseteq Q$ è l'insieme di stati finali.

Possiamo generalizzare $\delta$ da simboli a parole:
- $\hat{\delta}(q, \varepsilon) = q$
- $\hat{\delta}(q, wa) = \delta(\hat{\delta}(q, w), a)$ con $w$ parola, $a$ ultimo simbolo di $w$.
Applichiamo quindi in sequenza la funzione $\delta$ ad ogni simbolo di $w$.

Una parola $w \in A^*$ è **accettata** da $\mathcal{A}$ sse $\hat{\delta}(q_{0}, w) \in F$, cioè se uno stato finale più essere raggiunto da quello iniziale.

Il linguaggio accettato da $\mathcal{A}$ è l'insieme di parole accettate da $\mathcal{A}$. Questi linguaggio vengono chiamati **regolari**

## Automi a stati finiti non deterministici (NFA)

Un NFA $\mathcal{A}$ è una tupla $(Q,A,\delta, q_{0}, F)$ dove:
- $Q,A,Q_{0},F$ sono gli stessi di un DFA.
- $delta : Q \times A \rightarrow 2^{Q}$. Leggendo un simbolo da uno stato si possono raggiungere più stati.
- 
Possiamo generalizzare $\delta$ da simboli a parole:
- $\hat{\delta}(q, \varepsilon) = \{q\}$. Leggendo la parola vuota si rimane sullo stesso stato.
- $\hat{\delta}(q, wa) = \bigcup\limits_{p \in \hat{\delta}(q,w)}\delta(p, a)$ con $w$ parola, $a$ ultimo simbolo di $w$. Il risultato è l'insieme di stati raggiungibili leggendo il simbolo $a$ dall'insieme di stati raggiunti dopo aver letto la parola $w$.

Una parola $w \in A^*$ è **accettata** da $\mathcal{A}$ sse $\hat{\delta}(q_{0}, w) \cap F \neq \varnothing$, cioè se **almeno una** computazione raggiunge lo stato finale.

### Espressività degli automi

I DFA sono casi particolari di NFA, in cui da ogni stato possiamo raggiungere un insieme che contiene un solo stato leggendo un simbolo.

Gli NFA sono almeno tanto espressivi quando i DFA, e tutti i linguaggio definibili da NFA sono definibili con DFA: sono anch'essi regolari.

### Equivalenza di DFA e NFA (dimostrazione)

#todo

## NFA con $\varepsilon$-mosse