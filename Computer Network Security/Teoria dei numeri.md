
## Divisibilità
$b$ divide $a$ (indicato con $b | a$) se esiste un $m$ tale che $a=mb$, con $a,b,m$ numeri interi. Non c'è resto nella divisione.

- $a | 1 \rightarrow \pm 1$
- $a|b, b|a \rightarrow a = \pm b$
- Ogni $a \neq 0$ divide $0$
- $a|b, b|c \rightarrow a|c$

### Divisione intera
Dati un intero positivo $n$ ed un intero non negativo $a$, dividendo $a$ per $n$ si ottiene il quoziente intero $q$ ed il resto intero $r$, tali che 
$$a = qn + r$$

### Massimo comune divisore
Il più grande intero che divide $a,b$, indicato con $gcd(a,b)$.

## Modulo
Il resto nella divisione intera tra $a$ ed $n$, indicato con $a\mod n$.
Due interi $a,b$ sono **congruenti modulo $n$** se $(a\mod n) = (b\mod n)$.

## Numeri primi
Numeri che hanno come divisori solo $1$ e loro stessi: non possono essere scritti come prodotto di altri numeri.

Al limite i numeri primi $<n$, indicati con $\pi(n)$, sono $\frac{n}{\ln_n}$. La frequenza dei numeri primi decresce lentamente.

Due numeri sono **relativamente primi** se il loro GCD è $1$.

### Teorema fondamentale dell'aritmetica
Ogni numero può essere fattorizzato come
$$a = p_{1}^{e_{1}}\cdot p_{2}^{e_2}\cdot\dots\cdot p_{t}^{e_t}$$

dove $p_1<\dots<p_t$ sono numeri primi.

### Teorema di Fermat
Se $p$ è un numero primo e $a$ è un intero positivo, allora

$$a^{p-1} = 1 \mod p$$

### Teorema di Eulero
Generalizzazione del teorema di Fermat: se $a,p$ sono relativamente primi, allora 

$$a^{\varnothing(p)} = 1 \mod p$$