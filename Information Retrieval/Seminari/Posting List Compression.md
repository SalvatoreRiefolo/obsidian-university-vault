
Compressione indice:
1. Compressioni dati (documenti)
2. Compressione posting list

Posting list sono la componente più pesante dell'indice.

## Approccio generale

Usiamo come riferimento la posting list seguente:

$$L = \langle 3, 7, 11, 23, 29, 37, 41, \dots \rangle $$

Compressione standard (ad esempio, Huffman) non efficace, ogni elemento della posting list occorre una sola volta.
Dato che gli elementi di $L$ formano una sequenza crescente monotona, possiamo trasformare le posizioni in una sequenza di differenze tra gli elementi consecutivi

$$\Delta (L) = \langle 3, 4, 4, 12, 6, 8, 4, \dots \rangle $$

I vantaggi sono duplici:
1. gli elementi della sequenza occorrono più volte, possono dunque essere codificati con una singola codeword. Assegnando ai termini con frequenze più alte le codeword di lunghezza minore si cerca di raggiungere il limite definito dal teorema di Shannon.
2. gli elementi sono più piccoli e necessitano di meno bit per la rappresentazione.

Questo metodo può anche essere usato per posting list più complesse, come ad esempio in un **indice posizionale document-centric** nel formato $(d, f_{t, d}, \langle p_1, \dots, p_{f_{t, d}} \rangle )$, dove:

- $d$ è il document id 
- $f_{t, d}$ è il numero di occorrenze del termine $t$ nel documento $d$

Ad esempio, la posting list:

$$L = \langle 
(3, 2, \langle 157, 311 \rangle),
(7, 1, \langle 212 \rangle),
\dots 
\rangle $$

viene compressa in:

$$L = \langle 
(3, 2, \langle 157, 154 \rangle),
(4, 1, \langle 212 \rangle),
\dots 
\rangle $$

Metodi diversi possono essere usati per comprimere ogni lista (doc id, frequenza, posizioni) in quanto possono seguire distribuzioni di probabilità molto differenti.

## Metodi non parametrici

Non prendono in considerazione la distribuzione dei gap nella lista, ma assumono che gli elementi condividano delle proprietà (ad esempio, gap più piccole sono più frequenti di quelle più grandi).

### Codice unario

Un numero intero (positivo) $k$ è rappresentato come una sequenza di $k-1$ bit 0 seguita da un singolo bit 1. 
Il codice è ottimale se la distribuzione dei $\Delta$-valori ha una distribuzione *geometrica* della forma

$$Pr[\Delta = k] = (\frac{1}{2})^k$$

I gap tra posizioni della posting list solitamente non hanno questa distribuzione, che descrive la probabilità di trovare un gap di dimensione $k + 1$ **doppia** rispetto a quella di trovare un gap di dimensione $k$.

### Elias's $\gamma$ code
Il codice unario trova utilizzo nel codice $\gamma$ di Elias.
Questo codice è formato da due componenti:
1. il **selettore** contiene la lunghezza del corpo codificata in unario.
2. il **corpo** contiene l'intero $k$ codificato in binario.

*immagine*

Se il selettore ha valore $j$ per l'intero $k$, sappiamo che $2^{j-1} \leq k \lt 2^j$ $\Rightarrow$ Il $j$-esimo bit meno significante è sempre 1 e può essere omesso.

La lunghezza della codeword per un intero $k$ è di $2 \cdot \lfloor log_2(k) \rfloor + 1$.

Questa codifica è appropriata per la compressione di liste con gap piccole, ed è ottimale per sequenze di interi che seguono la distribuzione di probabilità:

$$Pr[\Delta = k] = \frac{1}{2 \cdot k^2}$$

### Codici $\delta$

Usano la stessa struttura dei codici $\gamma$. Nei codici $\delta$ le due componenti sono:
1. il **selettore** contiene la lunghezza del corpo in codifica $\delta$.
2. il **corpo** contiene l'intero $k$ codificato in binario.

La lunghezza della codeword per un intero $k$ è di $\lfloor log_2(k) + 2 \cdot \lfloor log_2(\lfloor log_2(k) \rfloor + 1)\rfloor + 1$.

Rispetto alla codifica $\gamma$ la codifica $\delta$ è efficiente il doppio per gap molto grandi; tuttavia tali gap sono rare e lo spazio salvato realisticamente è tra il 15% ed il 35%. 
Il codice è ottimale per sequenze di interi che seguono la distribuzione di probabilità:

$$Pr[\Delta = k] = \frac{1}{2k \cdot (log_2(k))^2}$$

### Codici $\omega$

Viene sfruttata nuovamente l'idea di usare un algoritmo di compressione per ridurre la dimensione del selettore. Un intero $k$ viene codificato usando il seguente algoritmo.

```
1. result = "0"
2. if k = 1 return result
3. result.prepend(bin(k))
4. k = floor(log(k))
5. goto 2
```

La lunghezza della codeword per un intero $k$ è di circa $2 + log_2(k) + log_2(log_2(k)) + \dots$

## Metodi parametrici

I codici non parametrici sono performanti quando i gap nelle posting list seguono una certa distribuzione. Tuttavia se la distribuzione è differente questi metodi possono portare a importanti sprechi di spazio.

I metodi **parametrici** esaminano le posting lists per estrarre informazioni utili da usare per una compressione migliore. Si possono dividere in:
1. *locali* (i parametri sono scelti diversamente per ogni lista o porzione della lista). Spesso danno risultati migliori ma per liste piccole l'overhead potrebbe essere eccessivo
2. *globali* (i parametri vengono usati per tutte le liste.). Questo porta a precisione minore ma a salvare (potenzialmente) più spazio in totale.

## Codici di Golomb/Rice

Supponiamo che i $\Delta$-valori di una posting list seguano una distribuzione geometrica della forma

$$Pr[\Delta = k] (1-p)^{k-1} \cdot p$$

con $k$ dimensione del gap.

Definiamo inoltre:
- $N$ = n° documenti
- $T$ = termine
- $N_T$ = n° documenti in cui appare $T$
- Probabilità di trovare T in un documento scegliendo casualmente nella collezione: $\frac{N_T}{N}$


Assumendo che i documenti siano indipendenti l'uno dall'altro, la probabilità di avere un gap di dimensione $k$ tra due occorrenze di $T$ successive è

$$Pr[\Delta = k] (1-\frac{N_T}{N})^{k-1} \cdot \frac{N_T}{N}$$

ossia, dopo aver incontrato un'occorrenza del termine $T$, incontriamo $k-1$ documenti senza $T$ prima di trovare un'altra occorrenza di $T$.

Ad esempio per $\frac{N_T}{N}$ = 0.01 (T appare nell'1% dei documenti), da questa distribuzione possiamo osservare che le probabilità maggiori sono date da interi la cui codifica binaria è compresa tra 6 e 8 bits.

La codifica che sfrutta queste caratteristiche per ottenere codici ottimali è definita come segue:

1. si sceglie un intero M
2. si divide il $\Delta$-valore $k$ in
	- $q(k) = \lfloor (k-1)/M \rfloor$ (quoziente)
	- $r(k) = (k-1)$ mod $M$ (resto)
3. Si codifica $k$ concatenando $q(k) + 1$ in unario e $r(k)$ in binario

La codifica in cui M è un intero arbitrario è chiamata *codice Golomb*, mentre quella dove M è una potenza di 2 è detta *codice Rice*.

Possiamo notare che, usando la distribuzione precedente ed un valore $M = 2^7$:
- Pochi gap sono più grandi di $2^8$ e richiedono più di 2 bit per il quoziente $\Rightarrow$ la maggior parte ne richiede meno di 3 per la codifica in unario.
- Pochi gap sono più piccoli di $2^5$ e richiedono al massimo 7 bit codificare il resto in binario.

### Scelta di M

Un codice C è ottimale se $|C(\sigma_1)| = |C(\sigma_2) + 1|\Rightarrow M(\sigma1) = \frac{1}{2}M(\sigma2)$ per due simboli $\sigma_1, \sigma_2$.

Inoltre, la codeword di Golomb per un intero k + M è 1 bit più lunga della codeword per k.


## LLRUN

L'idea è di raggruppare gap di dimensione simile in buckets e assegnare la stessa codeword di Huffman per tutti i valori nello stesso bucket: successivamente si effettua una codifica simile ai $\gamma$ code, dove il selettore è la codeword di Huffman. Di contro, bisogna anche trasmettere l'albero di Huffman per la codifica, che potrebbe avere fino a 160 bit.

## Context-Aware methods

Se si considera la dipendenza tra gli elementi delle posting list, si possono ottenere risultati migliori per la compressione: infatti a volte si possono trovare "cluster" dove lo stesso termine appare molte volte in poco testo.

## Finite-context LLRUN


