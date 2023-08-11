Ci si vuole assicurare che certe proprietà siano sempre (necessariamente)/a volte (possibilmente) vere nel futuro.

Si parla di **Linear Temporal Logic (LTL)** quando non si hanno ramificazioni nel futuro. La variante **Propositional LTL (PLTL)** aggiunge la logica proposizionale ai connettivi temporali.

## Sintassi
- $X$: "**next**", connettivo unario. L'argomento è $true$ al prossimo istante temporale.
- $U$: "**until**", connettivo binario. $p U q$ indica che deve esistere un momento nel futuro dove $q=true$ e fino a quel momento $p=true$.
- $F$: "**future**", connettivo unario. L'argomento è $true$ prima o poi nel futuro.
- $G$: "**global**", connettivo unario. L'argomento è $true$ sempre nel futuro.
- $F^{\infty}$: connettivo unario. L'argomento è $true$ infinite volte nel futuro.
- $B$: "**before**", connettivo binario. $p B q$ indica che $p=true$ in uno o più istanti precedenti a $q=true$.

$F,G$ sono **inter-definibili**: $Fp = \lnot G \lnot p$, $Gp = \lnot F \lnot p$.

## Condizioni di fairness in LTL
#esame 

Si definiscono con $En$ l'abilitazione di una transizione e $Tk$ la sua esecuzione. 

- **Compassione**: viene definita come $GFEn \rightarrow GFTk$: se una transizione è abilitata infinite volte nel futuro, allora è eseguita infinite volte nel futuro.
- **Giustizia**: viene definita come $G(GEn \rightarrow FTk)$: ovunque nel futuro, se una transizione è sempre abilitata prima o poi verrà eseguita.

## Strutture di LTL

Una **Linear Temporal Structure** è definita da una tripla $M=(S,X,L)$, dove:

- $S$ è l'insieme di stati.
- $X: \mathbb{N} \rightarrow S$ sequenza di stati.
- $L: S \rightarrow AP^{Q}$ una funzione di labeling.

La **verità** di una formula nella è struttura è definita come $M, x \models p$: in posizione $x$ della struttura $M$, $p=true$.

Per verificare la veridicità di una formula LTL si utilizzano i [[Tableau|tableau]].

## Model checking su LTL

Si pongono i seguenti problemi su $P,\varphi$.

- $P$-validità: dato un programma a stati finiti $P$ e una formula $\varphi$, questa è soddisfatta da tutte le [[Fair Transition System#Computazioni di un FTS|P-computazioni]]? Per determinare il problema si applica un algoritmo efficiente per vedere se esiste una $P$-computazione che soddisfa $\lnot p$.
- $P$-soddisfacibilità: dato un programma a stati finiti $P$ e una formula $\varphi$, esiste una $P$-computazione che soddisfa $\varphi$?

In entrambi i casi, si utilizzano i tableau per risolvere efficientemente il problema.

### Definizioni
- Per ogni atomo $A$ sia $state(A)$ la congiunzione di tutte le formule locali in $A$.
- L'atomo $A$ è **consistente** con lo stato $s$ se $s \models state(A)$, cioè se tutte le formule locali sono soddisfatte da $s$.
- Sia $\theta: A_0,A_1,\dots$ il percorso in $T_\varphi$ e sia $\sigma: s_0,s_1,\dots$ una computazione di $P$. $\theta$ è la **scia** (trail) di $T_\varphi$ su $\sigma$ se $A_j$ è consistente con $s_j$ per tutti i $j \geq 0$.
- Per ogni atomo $A \in T_\varphi$, $\delta(A)$ è l'insieme di successori di $A$ nel tableau.

## Grafo del comportamento
Dato un programma $P$ ed una formula LTL $\varphi$ si costruisce il **grafo del comportamento** di $(P,\varphi)$, chiamato $\mathcal B_{(P,\varphi)}$ come il **prodotto** del grafo di $P$ e del tableau di $\varphi$.

Il grafo risultante ha le seguenti componenti:
- **Nodi $(s,A)$** con $s$ stato di $P$ e $A$ un atomo consistente con $s$.
- **Arco $\tau$-labeled**: esiste tale arco tra $(s,A)$ ad $(s,A')$ se nel tableau sfrondato: 
	- $s' = \tau(s)$, cioè $s'$ è un successore di $s$.
	- $A' \in \delta(A)$, cioè $A'$ è un successore di $A$.
- **$\varphi$-nodi iniziali**: coppie $(s, A)$ dove $s$ è uno stato iniziale e $A$ è un atomo iniziale, ed $A$ è consistente con $s$.

Attraverso un algoritmo che aggiunge ad i nodi iniziali gli altri nodi connessi attraverso transizioni si crea il grafo del comportamento.

### Proprietà

Sia $\varphi$ una formula LTL.

La sequenza infinita $\pi: (s_0,A_0),(s_1,A_1),\dots$, con $(s_0,A_0)$ $\varphi$-nodo iniziale, è un percorso in $\mathcal B_{(P,\varphi)}$ se e solo se:
- $\sigma_{\pi}: s_0s_1\dots$ è una run (computazione a meno di fairness) di $P$.
- $\theta_{\pi}: A_0A_1\dots$ è una scia di $T_{\varphi}$ su $\sigma_{\pi}$ ($\forall j \geq 0$, $A_j$ è consistente con $s_j$).

Esiste una $P$-computazione che soddisfa $\varphi$ se e solo se esiste un percorso $\pi$ in $\mathcal B_{(P,\varphi)}$ che parte da un $\varphi$-nodo iniziale tale che:
- $\sigma_{\pi}: s_0s_1\dots$ è una computazione (fair run).
- $\theta_{\pi}: A_0A_1\dots$ è una scia soddisfacente (fulfilling trail) di $T_{\varphi}$ su $\sigma_{\pi}$.

## Grafo adeguato
#todo slides

## Traduzione di LTL+P in automi di Büchi

Si traducono formule LTL+P, con P rappresentazione del passato, in automi di Büchi.
A questo fine si definiscono il concetto di **operatori passati**:
- $Y\varphi$: **yesterday**, se la formula è valida ad un istante temporale $i$, era valida anche all'istante $i-1$.
- $\varphi_{1} s \varphi_{2}$: **since** se $\varphi_2=true$ ad un istante $i$, esiste un istante $i \leq j$ tale per cui $\forall j, \varphi_{1}= true$.

Il passato è **finito**.

### Teorema di Markley
LTL+P può essere esponenzialmente più succinto di LTL.
