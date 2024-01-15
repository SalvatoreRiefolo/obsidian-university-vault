---
tags: 
  - complexity-information-theory
---

La quantità di informazione trasmessa attraverso una certa stringa è correlata alla sua probabilità: più è bassa la probabilità che un evento accada, più è l'informazione che esso trasmette.

Una prima definizione può essere data da 

$$\frac{1}{P(e)}$$

dove $P(e)$ è la probabilità dell'evento $e$.

Quando $P(e) \rightarrow 1$ l'informazione tende a $1$, mentre quando $P(e) \rightarrow 0$ l'informazione tende a $\infty$.

Vorremmo però che l'informazione tenda a $0$ quando la probabilità tende a 1. Applicando la trasformazione logaritmica otteniamo che l'informazione può essere definita come

$$\log_2\frac{1}{P(e)} = \log_2 1- \log_2 P(e) = - \log_2 P(e)$$

In questo modo quando $P(e) \rightarrow 1$ l'informazione tende a $0$, mentre quando $P(e) \rightarrow 0$ l'informazione tende a $\infty$.

## Entropia di Shannon

L'entropia di Shannon definisce la quantità media di informazione data una distribuzione di probabilità. Per semplicità tutti i $\log$ usati a seguire saranno $\log_2$, se non diversamente specificato: questo perché la quantità di informazione viene data in bits.

$$H(P) = \sum_{i=0}^k p_i \cdot (-\log p_i) = -\sum_{i=0}^k p_i \cdot \log p_i$$ 

Esempi:
- Lancio di una moneta non truccata. Probabilità 0.5 per ogni lato, quantità di informazione trasmessa: 1 bit.
- Lancio di una moneta truccata. Probabilità $\neq$ 0.5 per ogni lato, quantità di informazione trasmessa: $\lt$ 1 bit
- $n$ lanci di una moneta non truccata. Probabilità 0.5 per ogni lato, quantità di informazione trasmessa dopo i lanci: $n$ bit.

### Proprietà

1. $H(P) \geq 0$
2. $H(P) = 0$ se un evento in $P$ ha probabilità 1
3. $H$ continuo in relazione a $P$
4. Se un evento è diviso l'entropia è additiva
5. $H(p_1, \dots ,p_n) < \log k = H(\frac{1}{k}, \dots ,\frac{1}{k})$, con $k = |A|$, $|A|$ = numero dei possibili eventi. Questa proprietà ci dice che l'entropia ha valore massimo quando le probabilità sono tutte uguali.

### Disuguaglianza di Jensen
Per dimostrare l'ultima proprietà si usa la disuguaglianza di Jensen: si considera una funzione concava di una certa forma e la combinazione lineare di due punti, e si usano per dimostrare la proprietà.

## Codifica

Dati due alfabeti 

$$A = \{ a_1, a_2, \dots , a_K \}$$
$$B = \{ b_1, b_2, \dots , b_D \}$$

una **codifica** è una funzione **iniettiva** che converte stringhe composte da elementi di $A$ in stringhe composte da elementi di $B$.

$$\varphi : A^* \rightarrow B^*$$

Esempi: 
- ASCII: codice blocco-blocco che converte caratteri dall'alfabeto di input in stringhe binarie di lunghezza fissa.
- "Plain binary": codice blocco-variabile che assegna caratteri dell'alfabeto di input in stringhe binarie di lunghezza variabile.

### Uniquely Decodable codes
Usare codici binari di lunghezza variabile può portare a stringhe non decodificabili: non è possibile capire a quale carattere dell'insieme $A$ appartiene una sottostringa del codice, anche se questo è iniettivo sui singoli caratteri.

Un codice **univocamente decodificabile** deve essere **iniettivo su tutte le stringhe di $A$**.
Un codice UD può avere delay: è necessario leggere i caratteri successivi a quello corrente per poter decodificare una sottostringa. Questo delay può essere potenzialmente illimitato (nel caso servano controlli di parità sui bits), ad esempio considerando che la decodifica possa avvenire su un flusso di dati real time.

### Prefix free codes

Una codifica è chiamata **prefix free** o **prefix** se data $\varphi,  \forall a_1,a_2 \in A$, $\varphi(a_1)$ non è un prefisso di $\varphi(a_2)$.
Se un codice è prefix free, allora è anche **UD senza delay**. Il codice può essere rappresentato come un albero $|B|$-ario che ha al massimo una label per ogni path: se più label sono presenti nello stesso path, queste sono una prefisso dell'altra.

I codici prefix free raggiungono gli stessi code rate dei codici UD con delay.

### Teorema di  Kraft-MacMillan per codici prefix free
Se $\varphi$ è UD, allora vale che

$$\sum_{i = 0}^k D^{-l_i} \leq 1$$

con $D = |B|$.
Intuitivamente, il teorema dice che se i $D^{-l_i}$ sono troppo grandi, o in altre parole se gli $l_i$ (lunghezze dei codici per carattere) **sono troppo piccoli** la disuguaglianza non è rispettata: questo accade quando codifichiamo con troppi pochi simboli, poiché sicuramente un codice sarà prefisso di un altro.

### Teorema direct
Data una scelta di $l_1,l_2,\dots ,l_k$ che soddisfa $\sum_{i = 0}^k D^{-l_i} \leq 1$, esiste un codice prefix free la cui lunghezza delle codifiche è $l_1,l_2,\dots ,l_k$.

La dimostrazione segue dal teorema di Kraft-MacMillan.

### Teorema di Shannon

Dati:
- $A$ alfabeto di input, $B$ alfabeto di output
- $P$ distribuzione di probabilità dei simboli di $A$
- $\varphi = A^* \rightarrow B^*$ codifica

definiamo la **lunghezza media della codifica**

$$EL(\varphi) = 
\sum_{i = 0}^k p_i \cdot len(\varphi(a_i)) =  
\sum_{i = 0}^k p_i \cdot l_i$$

Il **primo teorema di Shannon** (teorema del codice sorgente) per codici $\varphi$ UD è definito come

$$EL(\varphi) \geq H_D(P)$$

dove $H_D(P)$ è l'entropia di $P$ usando il logaritmo in base $D = |B|$.

Intuitivamente, la definizione da un limite inferiore alla lunghezza media di una codifica, che è data dall'entropia della distribuzione di probabilità relativa all'alfabeto di input. 
Si assume che la sorgente sia:
- Stazionaria: la distribuzione di probabilità $P$ dei simboli di $A$ non varia nel tempo
- Senza memoria: non c'è dipendenza tra i simboli inviati in due momenti diversi.

Il limite $H_D(P)$ è raggiungibile solo se ogni simbolo di $A$ ha la stessa probabilità: in ogni altro caso la lunghezza media è maggiore di $H_D(P)$.

### Codifica di Shannon
Dal primo teorema di Shannon abbiamo un limite inferiore alla lunghezza media della codifica. 

Dato che

$$EL(\varphi) = \sum_{i = 0}^k p_i \cdot l_i$$
$$H(P) = \sum_{i=0}^k p_i \cdot (-\log_D p_i)$$ 

è naturale pensare che se $l_i \approx -\log_D(p_i)$ allora la lunghezza media del codice sia la minore possibile, e che il codice sia **ottimale**.
È possibile dimostrare che un codice così definito sia prefix free usando il **teorema direct**.

Esempio: se le probabilità di un alfabeto $A = \{a,b\}$ sono $P = \{\frac{31}{32},\frac{1}{32}\}$, allora abbiamo che la lunghezza dei due codici binari è:

- $\varphi(a) = -\log_2(\frac{31}{32}) < 1 \rightarrow 1$
- $\varphi(b) = -\log_2(\frac{1}{32}) = 5 \rightarrow 5$

Tuttavia il codice è sub-ottimale: non raggiunge il limite inferiore dettato dall'entropia.

### Codifica di Shannon-Fano
Si divide l'alfabeto $A$ in due parti, cercando di avere probabilità 0.5 per i due sottoinsiemi.
Per far ciò prima si ordinano le probabilità in ordine decrescente, dopo di che si sommano dalla più piccola e si divide l'insieme quando la probabilità raggiunge 0.5.

### Codificare tuple
Codificando tuple si consuma più spazio per la rappresentazione del codice, ma si raggiungono code rate migliori. Il rapporto tra entropia (lunghezza ideale) e lunghezza media ci da l'**efficienza** di una codifica.

### Codifica di Huffman
Invece che creare l'albero per la codifica partendo dalla radice e dividendola, l'approccio di Huffman parte dalle foglie: si ordinano le probabilità e si uniscono le due più piccole, e si itera fino a che la somma delle probabilità non è uguale a 1. Questo codice è ottimale.

### LZ77
L'idea è di cercare il prefisso più lungo del buffer dei ricerca (che contiene parte del messaggio già scansionato) all'interno del look-ahead buffer (che contiene il resto del messaggio da codificare), in modo da codificare attraverso delle tuple contenenti l'offset della posizione corrente del prefisso corrispondente ed il numero di caratteri consecutivi che corrispondono, in modo sfruttare i pattern all'interno del messaggio per ottenere code rate migliori. Ottimale sotto alcune ipotesi.