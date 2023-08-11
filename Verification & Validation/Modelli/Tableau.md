
Un **tableau** $T_\varphi$ è un grafo diretto dove i nodi sono $\varphi$-atomi, es esiste un arco tra gli atomi $A,B$ se per ogni $Xp \in \Phi_\varphi$, $Xp \in A$ se e solo se $p \in B$. 
Se un atomo presenta un connettivo temporale *Next* su una proposizione o formula, quella proposizione o formula deve essere vera nell'atomo seguente.

### Regole di espansione
Si adottano delle regole di espansione dei connettivi temporali:

- $GP \approx P \land XGp$
- $FP \approx p \lor XFp$
- $pUq \approx q \lor (p \land X(pUq))$

### Chiusura di una formula $\varphi$
Con $\Phi_\varphi$ si indica la chiusura di una formula $\varphi$, il più piccolo insieme di formule che soddisfano:
- $\varphi \in \Phi_\varphi$, la formula stessa appartiene all'insieme
- Per ogni $p \in \Phi_\varphi$ e per ogni sotto-formula $q$ di $p$, $q \in \Phi_\varphi$. L'insieme contiene tutte le sotto-formule di tutte le formule.
- Per ogni $p \in \Phi_\varphi$, $\lnot p \in \Phi_\varphi$. L'insieme contiene tutte le sotto-formule negate.
- Per ogni $\psi \in \{Gp,Fp,pUq\}$, se $\psi \in \Phi_\varphi$ allora $X \psi \in \Phi_\varphi$. Per ogni formula comprendente un connettivo temporale nella formula, esiste anche il *next* di tale formula.

Ad esempio, la chiusura della formula $Gp \land F \lnot p$ è 
$$\{Gp \land F \lnot p, Gp, F \lnot p, \lnot Gp, \lnot F \lnot p, XGp, XF \lnot p, \lnot XGp, \lnot XF \lnot p, p, \lnot p\}$$

Per ogni formula $\varphi$ vale che $|\Phi_{\varphi}| < 4|\varphi|$. Questo perché dato un connettivo temporale, ad esempio $Gp$, le formule $Gp, \lnot Gp, XGp, \lnot XGp$ sono sempre presenti.

### Tabelle $\alpha$ e $\beta$
#todo pagina 89

## Validità nel tableau

### Atomo
Un **atomo** su $\varphi$, chiamato $\varphi$-atomo, è un insieme $A \subseteq \Phi_\varphi$ tale che la congiunzione di tutte le formule locali in $A$ siano soddisfacibili. 
Ad esempio $A = \{\varphi, Gp, F \lnot p, XGp, XF \lnot p, p\}$ è un atomo (nessuna formula contraddice le altre), mentre in $A = \{\varphi, \underline{Gp}, F \lnot p, \underline{\lnot XGp}, XF \lnot p, p\}$ le formule sottolineate si contraddicono.

Gli atomi sono i **nodi** del tableau, e rappresentano lo stato di un modello.

Un insieme $S \subseteq \Phi_\varphi$ è **mutualmente soddisfacibile** se esiste un modello $\sigma$ ed una posizione $j$ tali che ogni formula $p \in S$ sia vera in posizione $j$.
Per ogni insieme di formule mutualmente soddisfacibili esiste un $\varphi$-atomo tale che $S \subseteq A$.

L'obiettivo è usare gli atomi all'interno del tableau per controllare che un modello esista.

### Formule base
Una formula base è una proposizione $p$ o una formula della forma $Xp$. Se una formula base è in un atomo, allora la chiusura della formula è nell'atomo. Lo stesso vale per la sua assenza.

### Percorso indotto
Dato un modello $\delta$ di $\varphi$, il percorso infinito $\pi_{\delta}: A_0,A_1,\dots$ nel tableau $T_\varphi$ è **indotto da $\delta$** se per ogni $j \geq 0$ e per ogni $p \in \Phi_\varphi$, $(\delta, j) \Vdash p$ se e solo se $p \in A_j$.

Dati $\varphi$ e $T_\varphi$, per ogni $\delta$ di $\varphi$ esiste un percorso indotto.

### Promessa 
La formula $\psi$ promette la formula $r$ se $\psi$ ha una delle possibili forme:
- $Fr$
- $pUr$
- $\lnot G \lnot r$

$r$ sarà vero in qualche istante nel futuro.

Un modello $\sigma$ contiene infinite posizioni $j \geq 0$ tali che $(\sigma,j) \Vdash \lnot \psi \lor (\sigma, j) \Vdash r$. 
La formula $\psi$ (promessa) non è soddisfatta ad un certo istante o $r=true$ (promessa soddisfatta).

### Atomo soddisfacente
Un atomo $A$ **soddisfa** (fulfill) $\psi$ che promette $r$ se $\lnot \psi \in A$ o $r \in A$.

### Percorso soddisfacente
Un percorso $\pi$ in $T_\varphi$ è soddisfacente se per ogni promessa $\psi \in \Phi_{\varphi}$, $\pi$ contiene infiniti atomi $A_j$ che soddisfano $\psi$ ($\lnot \psi \in A_{j} \lor r \in A_j$).
Un percorso $\pi_\delta$ è indotto da un modello se e solo se è soddisfacente.
Una formula $\varphi$ è soddisfacibile se e solo se $T_\varphi$ contiene un percorso soddisfacente $\pi = A_1,A_2,\dots$ tale che $\varphi \in A_0$.

### Esistenza di un percorso soddisfacente

Un sotto-grafo $S \subseteq T_\varphi$ è **fortemente connesso** (SCS) se per ogni coppia di atomi distinti $A,B \in S$ esiste un percorso $A \rightarrow B$ che passa attraverso altri atomi di $S$.

Un **SCS transiente** è un SCS composto da un singolo nodo (atomo) non connesso a se stesso.
Un SCS non-transiente è **soddisfacente** se ogni formula $\psi \in \Phi_\varphi$ che promette $r$ è soddisfatta da qualche atomo $A \in S$ ($\lnot \psi \in A \lor r \in A$ o entrambe). 

Un SCS $S$ è **$\varphi$-raggiungibile** se esiste un percorso finito $B_0,\dots,B_k$ tale che $\varphi \in B_0$ e $B_{k} \in S$.
Il tableau $T_\varphi$ contiene un percorso soddisfacente se e solo se $T_\varphi$ contiene un SCS soddisfacente $\varphi$-raggiungibile.

Un SCS è **massimale** (MSCS) se non è contenuto in nessun SCS più grande.
Una formula $\varphi$ è soddisfacibile se e solo se il tableau $T_\varphi$ contiene un MSCS soddisfacente $\varphi$-raggiungibile. 
Si possono rimuovere preliminarmente tutti gli atomi che non sono raggiungibili da un $\varphi$-atomo.