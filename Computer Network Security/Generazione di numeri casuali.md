I numeri binari generati casualmente sono usati in crittografia per distribuire chiavi e in schemi di autenticazione, generare chiavi asimmetriche e generare flussi di bit per cifratura simmetrica.

Gli algoritmi usati sono deterministici e le sequenze non sono quindi veramente casuali, ma un buon algoritmo genera sequenze che passano molti test di casualità. I numeri così generati si definiscono **pseudorandom**.
Un generatore genera una sequenza di numeri pseudocasuale ricreabile utilizzando lo stesso input, chiamato **seed**. 

Le sequenze di numeri casuali generate devono essere:
1. **Casuali**: la frequenza di $0,1$ nella sequenza deve essere quasi la stessa, e non ci deve essere correlazione tra le sotto-sequenze.
2. **Non predicibili**: i numeri generati devono sembrare casuali, così come i numeri ancora da generare. La non predicibilità si divide in:
	- **Forward unpredictability**: se non si è a conoscenza del seed non è possibile capire qual è il prossimo bit della sequenza esaminando i bit precedenti
	- **Forward unpredictability**: non è possibile determinare il seed a partire dai valori generati. 

## Sorgenti di entropia per TRNG
Il seed usato dai PRNG deve essere casuale o pseudocasuale: tipicamente si usa un **true random number generator** per produrlo.
Un TRNG ha come input dei valori letti dall'ambiente, che tendono ad essere casuali. Ad esempio si possono usare input video/audio o le fluttuazioni nella velocità di un disco dovute alla resistenza dell'aria.

### Condizionamento
Le sequenze potrebbero contenere dei **bias**, cioè avere più $0$ rispetto ad $1$ o viceversa: si usano algoritmi per modificare i bits per aggiungere casualità ed aumentare l'entropia. Si usano a tale scopo le [[Funzioni di hash|funzioni di hash]].

### Health Tests
Si vuole controllare che la sorgente operi come ci si aspetta. Ad esempio si vuole determinare se la sorgente rimane bloccata su un singolo valore o ha una perdita notevole in entropia a causa di cambiamenti nell'ambiente.

## PRNG & PRF

Uno **pseudorandom number generator** produce un numero a partire da un seed.
Una **pseudorandom function** non genera un numero ma una sequenza di bit di lunghezza fissa.

Perché siano utili in crittografia, PRNG e PRF devono generare sequenze che non possono essere predette senza avere a disposizione il seed.

## Algoritmi per PRNG

### Linear Congruential Generator

Si definiscono:
- $m > 0$ il modulo.
- $a, 0<a<m$ il moltiplicatore.
- $c, 0\leq c<m$ l'incremento.
- $X_{0,}0 \leq X_{0}<m$ il seed.

La sequenza è definita come 

$$X_{n+1} = (a X_{n} + c) \mod m$$

La scelta di $a,c,m$ determina la qualità del generatore.

### Blum Blum Shub

Generatore di bit pseudocasuali **crittograficamente sicuro**.
La sequenza si produce con i seguenti step:
1. Si inizializza il generatore con un seed.
2. Si genera $r = x^{2} \mod n$. 
3. L'output è il bit meno significativo di $r$.
4. Si torna allo step 2. usando come input $x=r$.

### Modalità CTR & OFB per cifrari a blocco

Si usano i cifrari a blocco per generare la sequenza. Il periodo di cifratura (dopo quante iterazioni i numeri si ripetono) dovrebbe essere alto e le chiavi dovrebbero approssimare numeri casuali veri, avendo una distribuzione di $0,1$ quasi equa. Inoltre una chiave di almeno 128 bit è desiderabile.

- CTR inizializza il contatore e lo usa come input all'algoritmo di cifratura, producendo bit casuali. Si itera sommando incrementando il contatore.
- OFB cifra il valore iniziale, produce bit randomici e usa questi bit come input per le interazioni successive.

### RC4
Si basa sulle permutazioni casuali, ed è molto veloce da eseguire. Non utilizzato per scopi critici in quanto presenta delle vulnerabilità.

### Feedback Shift Register & Grain-128a
Una sequenza di celle di memoria da 1 bit. Ogni cella a una linea di output che indica il bit memorizzato, ed una linea di input. Durante gli istati dettati da un clock il valore di ogni cella è rimpiazzato con quello della linea di input.
Viene fatto uno shift del bit meno significativo, che viene restituito come output. Gli altri bit subiscono lo shift verso destra ed il nuovo bit più significativo è calcolato come una funzione degli altri bit nei registri.
La funzione è implementata come gate logico tra i vari registri, e può essere lineare o non lineare.

**Grain-128a** è un cifrario a flusso che sfrutta due FSR, uno con feedback lineare e uno non lineare.