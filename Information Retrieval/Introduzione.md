
L'**Information Retrieval** è la pratica di **trovare materiali** di natura **non strutturata** (testuale) che soddisfa un **bisogno di informazione** in una **collezione vasta**.

**Problema**: c'è troppa informazione che cresce in modo esponenziale.
I soli algoritmi non bastano per gestire e analizzare i risultati.

Rispetto alle **basi di dati**, l'information retrieval per mette di ottenere:
- dati parziali vs dati esatti
- informazioni incomplete vs complete
- risultati confusi vs precisi
- set rankati vs set normali
- rilevanza vs match 

## Punto di vista dell'utente
Il sistema agisce su una collezione di documenti (testuali, immagini, video...).
L'utente ha un bisogno di informazione: 
1. Invia una query
2. Ottiene set di risultati con un ranking (possono anche essere clusterizzati, etc...)
3. I risultati sono analizzati
4. Utente soddisfa il bisogno **OPPURE** cambia la query in base alle informazioni ottenute

## Punto di vista del sistema
Recupera documenti dalla collezione contenenti i **query terms** (termini contenuti nella query).
I query terms sono **pesati** (termini con poco significato sono trascurati, ad esempio articoli e congiunzioni).
Le collezioni su cui si effettua la ricerca sono **indicizzate**, cioè contengono puntatori che **collegano query terms a documenti**.

## Caratteristiche del bisogno di informazione
La ricerca è composta da: 
- **Argomento (topic)**: su cosa dovrebbero essere i documenti da ottenere.
- **Azione (task)**: cosa devo fare con l'informazione ottenuta.
- **Contesto (context)**: altre informazioni circa conoscenza pregressa, tempo a disposizione, posizione, etc...

In base al bisogno dell'utente, diverse metriche possono essere usate per valutare i risultati:
- **Precisione**: un risultato è molto preciso quando idealmente il primo documento ottenuto contiene tutte le informazioni di cui si ha bisogno.
- **Recall**: un risultato ha un alta recall se idealmente trova tutti i documenti relativi all'argomento della ricerca. È la percentuale di documenti rilevanti trovati rispetto a **tutti i documenti rilevanti nella collezione** (nota:  è impossibile anche attraverso indici trovare tutti i possibili documenti rilevanti $\Rightarrow$ "dark matter problem", documenti rilevanti non trovati).

Idealmente vorremmo che la precisione sia uguale alla recall: vengono trovato tutti i documenti rilevanti che risolvono precisamente il bisogno.

Non c'è un **intermediario**: nei sistemi classici (es: biblioteca) un intermediario offre aiuto di vario genere all'utente, ad esempio indirizzandolo verso i libri di cui ha realmente bisogno e offrendo un lessico adatto per la ricerca. 
Inoltre, inizialmente i sistemi di IR erano usati dall'intermediario e non direttamente dall'utente finale. L'intermediario possiede conoscenza, terminologia, conoscenza della collezione e tecniche per la ricerca e la comprensione della richiesta che l'utente non ha
Questo supporto non è direttamente offerto nell'IR.

Il bisogno di informazione può essere diviso in due livelli:
1. **Livello cognitivo**: l'utente dialoga con il sistema IR nel corso di diverse iterazioni, aggiustando la ricerca ad ogni nuova iterazione. Difficile lavorare a livello cognitivo per recuperare documenti (difficile comprendere il reale bisogno dell'utente).
2. **Livello sintattico**: vengono usati i termini della query per ricercare i documenti correlati. Facile da implementare ma non funziona sempre ("to be or not to be", tutti termini solitamente con poco peso all'interno della query che però racchiudono un significato preciso nella query specifica).

Spesso informazione necessaria $\neq$ informazione ottenuta. I documenti possono essere pertinenti ma possono non soddisfare il bisogno ('rilevanza' per l'utente è diversa da 'rilevanza' per il sistema).

Altri problemi:
- Mismatch di vocabolario: termini usati per la ricerca diversi da quelli usati dall'autore.
- Differenze nei termini: "online" vs "on-line"
- Differenze semantiche: sinonimi, termini ambigui, termini comuni in diverse lingue

## Obiettivo
Si vuole puntare al livello cognitivo/semantico (ciò che si intende) appoggiandosi al livello sintattico (come viene posta la richiesta) per ottenere informazioni che soddisfano un bisogno.

# Cosa NON è Information Retrieval

1. $\neq$ **Natural Language Processing**
2. $\neq$ **Risposta a domande** (IR restituisce documenti, non risposte)
3. $\neq$ **Database**: IR ritorna informazioni utilizzando una query semantica, i databases ritornano dati utilizzando una query che rispetta una sintassi. I documenti non sono strutturati ma sono "naturali"