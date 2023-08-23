- Un **gruppo** $G$ è un insieme di elementi con un'operazione binaria $\star$ che associa coppie di elementi $(a,b) \in G$ ad elementi $(a \star b) \in G$, tali che **chiusura**, **associatività**, **identità**. **inverso** e **commutatività** sono definiti per coppie di elementi e operazione. 
- Un **gruppo ciclico** è un gruppo per cui vale l'applicazione ripetuta dell'operatore. Ad esempio $a^{3}=a \star a \star a$
- Un **anello** è un gruppo per cui valgono anche **chiusura** e **associatività** per l'operazione di moltiplicazione, e le regole di **distributività**. Inoltre sono anche definiti la **commutatività della moltiplicazione**, **l'identità moltiplicativa** e l'impossibilità di svolgere **divisioni per zero**.
- Un **campo** $F$ è un insieme di elementi con due operazioni binarie, addizione e moltiplicazione, tale che i precedenti assiomi sono rispettati ed tale per cui esiste l'**inverso moltiplicativo**. In sostanza, possiamo svolgere somma e moltiplicazione (e di conseguenza differenza e **divisione**, quest'ultima non possibile negli altri gruppi) senza lasciare l'insieme.

## Campi di Galois

Il campo finito di ordine $p$ è definito come $GF(p^n)$, il campo con $p^n$ elementi.
Le operazioni vengono effettuate modulo $p^n$, in modo tale che il risultato rientri nell'insieme.

Un polinomio può essere riscritto nella forma 

$$f(x) = q(x)g(x) + r(x)$$

$q(x)$ è interpretabile come il quoziente, mentre $r(x)$ come il resto: segue che $r(x) = f(x) \mod g(x)$. Se un polinomio non può essere espresso come prodotto di due polinomi su $F$ allora il polinomio è **irriducibile** ed è anche chiamato **polinomio primo**.

Similarmente si può definire anche il **GCD polinomiale**: è il polinomio $c(x)$, divisore più grande di $a(x), b(x)$.

Usando coefficienti $0,1$, un polinomio può essere usato per rappresentare una stringa di bit.
L'addizione diventa l'operazione XOR, e la moltiplicazione diventa shift+XOR.

### Generatore
Un **generatore** $G$ di un campo $GF(q)$ è un elemento tale che le sue prime $q-1$ potenze generano tutti gli elementi di $F$

Una **radice** di un campo $F$, definito da un polinomio $fx$, è ogni elemento $b \in F$ se $f(b)= 0$.

Una radice $g$ di un polinomio primo è un generatore del campo $F$.