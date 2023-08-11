Gli OBDD rappresentano in modo efficiente formule booleane.

I nodi interni sono etichettati con una variabile $var(v)$ e hanno due successori: $low(v),high(v)$.
I nodi terminali sono etichettato da un valore $0$ o $1$.

L'ordinamento delle variabili impatta significativamente la forma dell'OBDD, seppur mantenendo l'equivalenza con altri OBDD generati dalla stessa formula: trovare un OBDD ottimale è un problema $NP$-completo.
Tuttavia applicando delle euristiche si possono ottenere buoni risultati: si raccomanda di mantenere vicine nell'OBDD le variabili che sono vicine nella formula.

### Rappresentazione di funzioni booleane
Una formula $f_v(x_1,\dots,x_n)$ è definita come:
1. se $v$ è terminale, se il suo valore è $1 \rightarrow f_v(\dots)=1$ , altrimenti $f_v(\dots)=0$.
2. se $v$ è interno con $var(v) = x_i$, $f_{v(\dots)}= \lnot x_{i} \land f_{low(v)}(\dots) \lor f_{high(v)}(\dots)$.

### Espansione di Shannon

Una formula $f$ può essere **espansa** nel seguente modo:

$$f= (\lnot x \land f|_{x\leftarrow 0}) \lor (x \land f|_{x\leftarrow 1})$$

## Operazioni logiche

- **Restrict**: questa funzione ausiliaria restringe una variabile $x_i$ della funzione ad una costante $b \in \{0,1\}$. $f|_{x_i\leftarrow b}(x_1,\dots,x_i,\dots,x_n)= f(x_1,\dots,b,\dots,x_n)$. La restrizione potrebbe portare l'OBDD in forma non canonica: ulteriori semplificazioni potrebbero essere necessarie.
- **Apply**: si applica l'operazione $*$ per comporre le funzioni $f,f'$. Siano $v,v'$ le radici di $f,f'$ e $x = var(v),x'=var(x')$ i loro valori.
	- Se $v,v'$ sono terminali, si applica $*$ ad i loro valori: $x * x'$.
	- Se $x = x'$ si usa l'espansione di Shannon: $f*f' = (\lnot x \land (f|_{x\leftarrow 0} * f'|_{x\leftarrow 0})) \lor (x \land (f|_{x\leftarrow 1} * f'|_{x\leftarrow 1}))$
	- Se $x < x'$ (cioè $x = 0, x' = 1$) allora $f'|_{x\leftarrow 0} = f'|_{x\leftarrow 1} = f'$: $f*f' = (\lnot x \land (f|_{x\leftarrow 0} * f')) \lor (x \land (f|_{x\leftarrow 1} * f'))$. L'operazione è simmetrica per $x > x'$.
- **Negazione**: data una formula $f$, l'albero per la formula $\lnot f$ è lo stesso ma con le foglie invertite.

Utilizzando Apply, si creano un numero esponenziale di sotto-problemi rispetto al numero di variabili nella funzione: ogni sotto-problema può generare due sotto-problemi.

È possibile evitare il costo esponenziale effettuando **caching**. Ogni sotto-problema corrisponde ad una coppia di OBDD, i sotto-grafi dell'OBDD originale. Ogni sotto-grafo è univocamente identificato dalla sua radice, quindi il numero di sotto-grafi distinti è limitato dalla dimensione degli OBDD di $f,f'$, cioè $O(|f| \cdot |f'|$. La complessità è quindi al più **quadratica** rispetto alla dimensione delle formule.
Prima di risolvere un sotto-problema, si accede alla cache con le radici delle due sotto-formule: se il sotto-problema è già stato risolto si riutilizza il risultato ottenuto.

## Regole di riduzione

Un OBDD in forma canonica si ottiene applicando le seguenti regole di riduzione:

1. Si rimuovono i nodi terminali duplicati, lasciando solo due nodi terminali $0,1$.
2. Si rimuovono i sottoalberi duplicati: se $var(v) = var(u) \land low(v) = low(u) \land high(v) = high(u)$, si elimina uno dei due nodi (con l'intero sottoalbero) e si **reindirizza** l'arco entrante alla radice del sottoalbero rimasto.
3. Si rimuovono i test ridondanti: se i due sottoalberi con radice $v$ sono identici, se ne rimuove uno e si reindirizza.
4. Per ogni nodo che punta ad un nodo $w$ la cui variabile $x_i$ è stata **ristretta**, si rimpiazza $w$ con il suo sottoalbero radicato in $low(w)$ se $x_{i} = 0$, e con $high(w)$ se $x_{i} = 1$.
