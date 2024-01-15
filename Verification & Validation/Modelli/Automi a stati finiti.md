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

Un DFA $\mathcal{A}$ è una quintupla $(Q,A,\delta, q_{0}, F)$ dove:
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

Il linguaggio accettato da $\mathcal{A}$ è l'insieme di parole accettate da $\mathcal{A}$, $L(\mathcal{A})$. Questi linguaggi vengono chiamati **regolari**.

## Automi a stati finiti non deterministici (NFA)

Un NFA $\mathcal{A}$ è una quintupla $(Q,A,\delta, q_{0}, F)$ dove:
- $Q,A,Q_{0},F$ sono gli stessi di un DFA.
- $delta : Q \times A \rightarrow 2^{Q}$. Leggendo un simbolo da uno stato si possono raggiungere più stati. $2^Q$ è il powerset di $Q$, ossia l'insieme di tutti gli insiemi di $Q$.

Possiamo generalizzare $\delta$ da simboli a parole:
- $\hat{\delta}(q, \varepsilon) = \{q\}$. Leggendo la parola vuota si rimane sullo stesso stato. 
- $\hat{\delta}(q, wa) = \bigcup\limits_{p \in \hat{\delta}(q,w)}\delta(p, a)$ con $w$ parola, $a$ ultimo simbolo di $w$. Il risultato è l'insieme di stati raggiungibili leggendo il simbolo $a$ dall'insieme di stati raggiunti dopo aver letto la parola $w$.

Una parola $w \in A^*$ è **accettata** da $\mathcal{A}$ sse $\hat{\delta}(q_{0}, w) \cap F \neq \varnothing$, cioè se **almeno una** computazione raggiunge lo stato finale.

### Espressività degli automi

I DFA sono casi particolari di NFA, in cui da ogni stato possiamo raggiungere un insieme che contiene un solo stato leggendo un simbolo.

Gli NFA sono almeno tanto espressivi quando i DFA, e tutti i linguaggio definibili da NFA sono definibili con DFA: sono anch'essi regolari.

### Equivalenza di DFA e NFA (dimostrazione)

Dato un NFA $\mathcal{A}$ il corrispondente DFA $\mathcal{A'} = (Q', A, \delta', q_0', F')$ è costruito nel seguente modo:

- $Q' = 2^Q$, il powerset degli stati di $\mathcal{A}$.
- $q_0' = {q_0}$, l'unico stato iniziale.
- $\forall P \in Q', \forall a \in A$ vale che $\delta'(P,a) = \delta(P,a)$.
- $F' = \{P \in Q' : P \cap F \neq \varnothing\}$ gli insiemi di stati che intersecati con gli stati finali di $\mathcal{A}$ contengono qualche stato.

Si vuole dimostrare che lo stato restituito dalla funzione di transizione applicata alla parola $w$ e allo stato $q_0$ da $\mathcal{A'}$ coincide con con l'insieme di stati restituito sulla stessa parola partendo dallo stato iniziale da $\mathcal{A}$.

Se $|w| = 0$ la funzione restituisce in entrambi i casi lo stato $\{q_0\}$.

Supponiamo che la tesi valga per parole di lunghezza fino a $n$. Consideriamo la parola $wa$ di lunghezza $n+1$: per definizione di FA, $\delta'(q_0,wa) = \delta'(\delta'(q_0, w), a)$. Sfruttando l'ipotesi, $\delta'(q_0,wa) = \delta'(\delta(q_0, w), a) = \bigcup_{p\in\delta(q0, w)} \delta'(\{p\}, a) = \bigcup_{p\in\delta(q0, w)} \delta(p, a) = \delta(q_0, wa)$.

$\mathcal{A'}$ accetta $w$ se e solo se $\delta'(q_0',w) \in F'$. Questo accade se $\delta(q_0,w) \in F'$, cioè se $\delta(q_0,w) \cup F \neq \varnothing$. Ciò vale se e solo se $\mathcal{A}$ accetta $w$. 
Deriva che $L(\mathcal{A}) = L(\mathcal{A'})$.

Il numero di stati del DFA è esponenziale rispetto al numero di stati del corrispondente NFA.

## NFA con $\varepsilon$-mosse
Un NFA con $\varepsilon$-mosse $\mathcal{A}$ è una tupla $(Q,A,\delta, q_{0}, F)$ dove:
- $Q,A,Q_{0},F$ sono gli stessi di un NFA.
- $\delta : Q \times A \cup \{\varepsilon\} \rightarrow 2^{Q}$. Una **$\varepsilon$-mossa** permette di eseguire una mossa senza consumare un simbolo. L'insieme di stati che può essere raggiunto da un certo stato $q \in Q$ è chiamato **$\varepsilon$-chiusura** (closure).

Per $P \subseteq Q$ definiamo $\varepsilon$-closure$(P) = \bigcup\limits_{p \in P} \varepsilon$-closure$(p)$.

Possiamo generalizzare $\delta$ da simboli a parole:
- $\hat{\delta}(q, \varepsilon) = \varepsilon$-closure$(q)$. Si avanza fino alla closure di $q$.
- $\hat{\delta}(q, wa) = \varepsilon$-closure$(\bigcup\limits_{p \in \hat{\delta}(q,w)}\delta(p, a))$ con $w$ parola, $a$ ultimo simbolo di $w$. Il risultato è l'insieme di stati raggiungibili leggendo il simbolo $a$ dall'insieme di stati raggiunti dopo aver letto la parola $w$.

>[!Teorema]
>Per tutti gli NFA con $\varepsilon$-mosse $\mathcal{A}$ esiste un NFA $\mathcal{A'}$ tale che $L(\mathcal{A}) = L(\mathcal{A'})$ e vice versa.
>Questo NFA è uguale all'NFA con $\varepsilon$-mosse se la $\varepsilon$-chiusura dello stato iniziale non porta in uno stato finale. In caso contrario, lo stato iniziale viene aggiunto agli stati finali: $F' = F \cup \{q_0\}$.
>
>Segue che NFA $\equiv$ DFA $\equiv$ NFA con $\varepsilon$-mosse.

