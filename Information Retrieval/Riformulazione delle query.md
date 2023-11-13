Tecniche per modificare la query aggiungendo, rimuovendo o rimpiazzando i termini ed i loro pesi.
La riformulazione può essere:
- **Manuale** o **semiautomatica**, in cui il sistema propone suggerimenti ma l'utente deve applicarli. Sono normalmente difficili da usare, in quanto i termini proposti potrebbero sembrare non correlati alla ricerca.
- - **Automatica**, in cui il sistema propone una query modificata.

## Feedback di rilevanza

Mentre l'utente esamina i documenti fornisce del **feedback** sulla loro rilevanza; il sistema può usare queste informazioni per proporre suggerimenti o cambiare il peso dei termini. 

In spazio vettoriale, il feedback muove il vettore verso i documenti più rilevanti, il **cluster di rilevanza**. Si suppone infatti che i documenti "vicini" in questo spazio abbiano rilevanza simile per la stessa richiesta. Il cluster potrebbe non essere unico o molto sparso.

### Nel modello probabilistico

Il feedback di rilevanza è alla base del [[3. Modello probabilistico#Modello probabilistico (Binary Independence Model)|modello probabilistico]]. Tuttavia non permette di aggiungere nuovi termini alla query ma solo di cambiare i pesi esistenti.

## Derivazione di nuove query

Si vuole passare da una query $q$ a $q'$, cercando di spostarsi verso il cluster dei documenti rilevanti e allontanarsi da quelli irrilevanti.

Si usano 3 formule che prendono in considerazione il numero di documenti recuperati e giudicati come rilevanti e non rilevanti per elaborare la nuova query. Questo formule sono particolarmente efficaci.

L'efficacia è calcolata sui documenti recuperati escludendo quelli già valutati. Il punteggio della query sarà più basso ma fornisce informazioni più utili per la comparazione delle tecniche.
Tuttavia non si possono completamente eliminare i documenti giudicati come rilevanti dai risultati: questo problema è da affrontare dal punto di vista dell'interfaccia utente.

## Espansione della query

Si vogliono identificare i termini più vicini ai termini usati in $q$ per effettuare l'**espansione della query**.

### Matrice di co-occorrenza

Data la matrice $M$ le cui righe sono i termini e le colonne sono i documenti in cui appaiono, la matrice $M \cdot M^T$ è la matrice di **co-occorrenza**, cioè la matrice la cui diagonale esprime quanti documenti contengono un dato termine.

Durante la moltiplicazione, se almeno uno dei due documenti non contiene un certo termine il valore da sommare è 0, altrimenti viene aggiunto 1. 

L'ipotesi afferma che se due termini sono co-occorrenti allora sono semanticamente associati, e la forza dell'associazione è determinata dai valori di $M \cdot M^T$.

### Analisi locale
I documenti ottenuti vengono analizzati quando la query è effettuata e sono usati per scegliere i termini per l'espansione in modo automatico. Vengono usate le radici dei termini e non i termini interi per cercare i termini simili.

Si usano due tecniche:
1. **Clustering locale**: si costruisce un cluster di termini correlati ai singoli termini della query e si aggiungono a $q$ i termini dei cluster. Si possono usare diversi metodi per il clustering, che si basano su una matrice di co-occorrenza ristretta ai documenti ottenuti (**cluster associativi**), sulla distanza dei termini nei documenti per la decidere il peso della co-occorrenza (**cluster metrici**) e sul vicinato (se due termini co-occorrono con un terzo termine, sono co-occorrenti tra di loro) dei termini (**clustering scalare**).
2. **Analisi del contesto locale**: lavora su **concetti** e non su termini. Si decompongono i documenti in passaggi di lunghezza fissa e si ordinano come fossero documenti. Si selezionano i primi $n$ e per ogni concetto $c$ in questi si calcola la similitudine $sim(q,c)$ tra query e concetto. Infine si selezionano i primi $m$ concetti e si aggiungono a $q$, con pesi inferiori a quelli dei termini originali. Questo metodo è efficace ma richiede che i concetti vengano definiti per la collezione specifica.

### Analisi globale
Si usano informazioni anche dell'intera collezione. Si costruiscono strutture al momento dell'indicizzazione per rappresentare le relazioni tra i termini. 
La struttura viene poi usata durante le query per aggiungere nuovi termini e ricalcolare i pesi

Per migliorare l'approccio si usano due tecniche:
1. **Tesauro di similarità**: si costruisce uno spazio vettoriale per i termini, dove ogni termine è un vettore e le dimensioni sono i concetti (i documenti): si inverte il modello vettoriale già visto. Si vuole trovare i documenti a cui un termine può appartenere ed in che misura. Si favoriscono i documenti brevi (termini sono più simili se occorrono in un corpo più breve).
2. **Tesauro statistico**: si usano cluster di documenti per creare cluster di termini. Se un cluster di documenti è abbastanza piccolo ed i documenti sono molto simili si selezionano i termini con la più bassa frequenza nella collezione e si crea un cluster con essi. Questi sono i termini che aiutano a discriminare maggiormente.