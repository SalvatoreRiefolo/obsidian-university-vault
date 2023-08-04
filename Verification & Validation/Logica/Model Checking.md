Dati:
- Una **configurazione del sistema** (modello o struttura $S$). Questa è rappresentabile tramite un automata, cioè una sequenza infinita di azioni su un $\Omega$-linguaggio (insieme infinito di parole).
- Una specifica (formula $\varphi$). Ad esempio "$x$ or $y$ until $a$".

Si vuole controllare che il modello **soddisfi** la specifica: si vuole dare risposta $yes/no$ in base a se $\varphi$ regge su $S$: $S \models \varphi$.

Una prima soluzione è compilare la negazione della specifica $\lnot \varphi$, tradurre la formula in automata e controllare che l'intersezione con l'automata del modello è vuota: se non lo è, allora esiste una computazione non valida.

## Logica

Una logica è composta da
- Un **vocabolario** (insieme di simboli)
- Una **sintassi** (le regole con cui usare i simboli)
- Una **semantica** (il significato)

Si useranno:
1. **1st order logic**: rappresenta singole entità
2. **2nd order logic**: rappresenta insiemi di entità
	- **Monadic**: insiemi di elementi
	- **Non-monadic**: insiemi di tuple di elementi

Una logica **monadica** è una logica unaria.

## Logica proposizionale

La logica proposizionale è composta da
- Vocabolario $\Sigma = \{p,q,\dots\}$ variabili booleane, $\lor,\land,\lnot,\rightarrow, \leftrightarrow$ connettivi.
- Semantica $S: \Sigma \rightarrow \{true,false\}$, "$S$ mappa variabili in $\Sigma$ a $\{true,false\}$"

$S \models p$ se e solo se $S(p) = true$.

### Algoritmi

$ModelChecking(\varphi, S)$ è un problema in $P$. Se la formula è composta da una sola variabile, si calcola $S(P)$, altrimenti si applica il controllo ricorsivamente sulle sotto-formule.

$Satisfiability(\varphi)$ è un problema in $EXP$ (deterministico), $NP$ (non-deterministico): bisogna tentare $ModelChecking$ su ogni possibile interpretazione.

### Economia di sintassi

1. Ogni formula è equivalente ad una senza $\rightarrow, \leftrightarrow$.
2. Ogni forma è equivalente a una in **Negation Normal Form**: questa formula ha le negazioni davanti alle singole variabili ed è calcolabile in tempo lineare se non sono presenti $\leftrightarrow$.
3. Ogni formula è **equi-soddisfacibile** ad una in **Conjunctive Normal Form**: questa formula è composta da clausole congiunte ($\land$) tra loro ed è calcolabile in tempo lineare se non sono presenti $\leftrightarrow$ applicando distribuzioni di variabili.

$CNF-SAT$ è comunque $NP-completo$.

### Tableau

Si cerca di determinare la soddisfacibilità di una formula "indovinando" pezzi della formula.
Data una formula NNF si parte dalla formula $\varphi$ e si seleziona per un congiunto una variabile, mantenendo le altre. Si continua ricorsivamente finché non viene trovata una foglia **non bloccata** (non contiene un letterale e la sua negazione) con solo letterali.

## Quantified Boolean Formulas (QBF)

La logica QBF è composta da
- Vocabolario $\Sigma = \{p,q,\dots\}$ variabili booleane, $\lor,\land,\lnot,\rightarrow, \leftrightarrow$ connettivi, $\forall, \exists$ quantificatori.
- Semantica $S: \Sigma \rightarrow \{true,false\}$, "$S$ mappa variabili in $\Sigma$ a $\{true,false\}$". $S \models \exists / \forall _p \varphi$ se e solo se $S' \models \varphi$ per qualche/ogni $S' \in \{S[P: true], S[P: false]\}$.

$S$ è una funzione che mappa le variabili *free* (non quantificate) in $true,false$: $S: FreeVars(\varphi) \rightarrow \{true,false\}$. 
Le variabili free di una formula $\varphi$ sono indicate come $\varphi(p_1,\dots,p_n)$

### Algoritmi

$ModelChecking(\varphi, S)$ è un problema $PSPACE$-completo. Per le variabili quantificate, dobbiamo richiamare la procedura con ciascuna variabile sia $true$ che $false$, e congiungere i risultati nel caso del quantificatore universale e disgiungerli per quello esistenziale.
Se la formula è in PNF con $n$ alternazioni di quantificatori, allora il problema appartiene a $\Sigma_n$ o $\Pi_n$, seguendo la [[4. Riduzioni#Gerarchia polinomiale|gerarchia polinomiale]].

$Satisfiability(\varphi)$ è riducibile a $ModelChecking$ e ha la stesa complessità. Nel caso di quantificatori universali, dobbiamo controllare entrambi i figli di tutti i nodi della variabile quantificata nell'albero delle esecuzioni. 
Se tutte le variabili sono quantificate esistenzialmente, allora il problema diventa $NP-completo$ (come per la logica proposizionale, che implicitamente quantifica esistenzialmente tutte le variabili).

### Economia di sintassi

1. Ogni formula è equivalente ad una in *Prenex Normal Form (PNF)*: si muovono i quantificatori all'inizio della formula e si applicano rinomine.

## First Order Logic (FOL)

La logica QBF è composta da
- Vocabolario $\Sigma = \{p,q,\dots,R,S,T\}$ variabili booleane e simboli relazionali, $\lor,\land,\lnot,\rightarrow, \leftrightarrow$ connettivi, $\forall, \exists$ quantificatori, $x,y,z$ variabili di primo ordine.
- Semantica composta da un universo $U^S$ e interpretazioni $R \rightarrow R^{S}\subseteq U^{S}\times \dots \times U^{S}$.

### Algoritmi

$ModelChecking(\varphi, S)$ è un problema $PSPACE$-completo. Rispetto alla complessità delle logiche precedenti, bisogna controllare che se la sotto-formula considerata è una relazione bisogna controllare che le variabili la soddisfino, e che per le variabili di primo ordine quantificate esista un assegnamento nell'universo che soddisfi la formula.

$Satisfiability(\varphi)$ è indecidibile, ed è possibile dimostrarlo riducendo il problema da $Domino$.
