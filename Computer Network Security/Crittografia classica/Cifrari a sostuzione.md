
I caratteri del plaintext sono sostituiti con altri caratteri.

## Cifrari monoalfabetici e criticità

In un cifrario **monoalfabetico** ogni carattere è sostituito con un altro carattere dell'alfabeto, e nel caso di 26 caratteri ci sono $26!$ sequenze utilizzabili come chiave.

Questo tipo di cifrario è semplice da attaccare poiché le frequenze delle lettere nel messaggio originale sono preservate, e tramite analisi statistica delle occorrenze è possibile dedurre la chiave più facilmente.

### Cifrario di Cesare
Cifrario usato su messaggi testuali. Si rimpiazza ogni lettera con la lettera tre (generalizzando, $n$) posizioni in avanti nell'alfabeto, tornando all'inizio se si supera l'ultima lettera.

$$E(k,p) = (p + k) \mod 26$$
$$D(k, c) = (c-k)\mod 26$$

dove $k$ è lo spostamento, $p$ è il carattere e $26$ è la lunghezza dell'alfabeto.

### Cifrario di Playfair

Si può affrontare il problema dei cifrari monoalfabetici dando più sostituzioni per una lettera singola, accorpando lettere tra loro in **digrammi** o **trigrammi**: ad esempio $th$ viene codificato con due caratteri, e $the$ con tre caratteri differenti.

Il cifrario di Playfair tratta i digrammi del testo come unità singole e le traduce in digrammi del testo cifrato. Utilizza una matrice $5 \times 5$ per cifrare e decifrare partendo da una parola chiave.

### Cifrario di Hill

Nasconde completamente le frequenze di caratteri singoli e digrammi.

Come gli altri cifrari precedenti, è vulnerabile ai **known plaintext attack**: dato un testo conosciuto, è facile capire dal testo cifrato in che modo avviene la sostituzione.

## Cifrari polialfabetici

Utilizzano più cifrari monoalfabetici, scegliendo in base ad una chiave quale cifrario usare per una certa trasformazione.

### Cifrario di Vigenère
Utilizza 26 cifrari di cesare con spostamento da 0 a 25. Ogni cifrario è identificato da un carattere, quello sostituito nel testo dato in input il carattere $a$.

Data una chiave lunga quanto il messaggio (si può ripetere una parola fino alla lunghezza desiderata) si applica il cifrario legato al carattere della chiave al corrispondente carattere del testo in chiaro.

La chiave può anche essere generata dando una chiave corta e concatenando a questa il plaintext mentre viene cifrato/decifrato: questo sistema si chiama **autokey**.

### Enigma
Utilizza più chiavi rispetto al cifrario di Vigenère.

### One-Time Pad
Si usa una stringa casuale come chiave per cifratura/decifratura: finché la chiave rimane sicura, questo schema **non è attaccabile**. Il testo prodotto non ha nessuna relazione statistica al testo in chiaro.

È l'unico sistema che offre **perfetta segretezza**.

## Cifrari a trasposizione

### Cifrario rail fence
Il testo viene scritto come una sequenza di diagonali e letto come sequenza di righe.

### Cifrario a trasposizione di righe

Si scrive il messaggio in un rettangolo riga per riga, e si legge colonna per colonna dopo aver permutato l'ordine delle colonne. La chiave è l'ordine della permutazione.