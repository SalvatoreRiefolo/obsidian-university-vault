Sono un modo per modellare [[Sistemi reattivi|sistemi reattivi]].

Un FTS è rappresentato come un grafo in cui i nodi sono **stati**, un insieme di variabili $V$ con tipo all'interno di un vocabolario.
Lo stato corrente è dato assegnando variabili ad un nodo. Ad esempio $x = \textbf{stato corrente}, x' = \textbf{stato successivo}, x,x' \in V$.

Un FTS può essere descritto attraverso [[Model Checking#First Order Logic (FOL)|logica del primo ordine]]: i **termini** sono costruiti sulle variabili $v \in V$ e su costanti, [[1. Logic Programming#Formule atomiche (atomi)|formule atomiche]] e formule di stato costruite con atomi e connettivi/quantificatori.
Uno stato è un'interpretazione che assegna ad ogni variabile $u \in V$ un valore $s[u]$ appartenente al dominio di $u$. L'insieme di tutti gli stati è $\Sigma$ e può essere infinito.

## Definizione di Fair Transition System
#esame

Un FTS è una quintupla $\langle V, \theta, \mathcal{T}, \mathcal{J}, \mathcal{C}\rangle$, dove:
- $V = \{u_1,\dots,u_n\}$ variabili di sistema. Sono partizionate in variabili $data$ e $control$, che controllano lo stato dell'esecuzione (cursori).
- $\theta$ è una formula di stato soddisfacibile che descrive l'insieme di stati iniziali, **la condizione iniziale**. Uno stato che soddisfa $\theta$ è uno stato iniziale.
- $\mathcal{T}$ è un insieme finito di transizioni, che rappresentano gli archi nel grafo del FTS. Ogni transizione $\tau \in \mathcal{T}$ è una funzione $\tau : \Sigma \rightarrow 2^\Sigma$, che mappa ogni stato un un insieme di stati $\tau(s) \subseteq \Sigma$. Ogni stato in $\tau(s)$ è un $\tau$-successore di $s$, e $\tau$ è **abilitata** se $\tau(s) \neq \varnothing$, cioè se la transizione applicata ad uno stato ha successori. Altrimenti, è **disabilitata**.

Una tripla $\langle V, \theta, \mathcal{T}\rangle$ rappresenta un sistema di transizioni **classico**. Aggiungendo le condizioni di *justice* e *fairness* si ottiene un sistema *fair*.

- $\mathcal{J} \subseteq \mathcal{T}$ sono le transizioni per cui richiediamo **giustizia** (justice), anche chiamata *weak fairness*. Si esclude l'esistenza di computazioni che sono sempre abilitate da un certo stato in avanti ma mai eseguite: prima o poi, se una transizione è abilitata, **deve** essere eseguita. Se una transizione viene abilitata e successivamente disabilitata senza essere eseguita non si viola il principio di giustizia. Questa è una proprietà che si vorrebbe applicare a (quasi) tutte le transizioni.
- $\mathcal{C} \subseteq \mathcal{T}$ sono le transizioni per cui richiediamo **compassione** (fairness), anche chiamata *strong fairness*. Si esclude l'esistenza di computazioni che sono abilitate un **infinito numero di volte** ed eseguite un **numero finito di volte**. Non deve esserci nessuna "ultima esecuzione" per queste transizioni. Questa proprietà è applicata ad un numero piccolo di transizioni.

La fairness di un sistema **non può** essere controllata su un frammento finito delle esecuzioni del sistema.
Giustizia e compassione sono anche chiamati **fairness requirements**.

## Rappresentazione delle transizioni

Le **transizioni** $\tau \in T$ possono essere rappresentate come formule FO: $\varphi_{\tau}(V,V')$, chiamate **relazioni di transizione**. 
Sono l'insieme di stati $V'$ raggiungibili da $V$. 
La transizione mettere in relazione uno stato $s$ ed i suoi $\tau$-successori $s' \in \tau(s)$.

>[!Esempio]
>Si rappresentano come $x,y,z,\dots$ i valori di variabili in $s$ e $x',y',z',\dots$ i loro valori in $s'$.
>La formula $x' = x + 1$ dice che il valore di $x$ allo stato $s'$ ($s'[x]$) è uguale al valore di $x$ più uno ($S[x]$ + 1).

La **condizione di abilitazione** di una transizione $\tau$ è definita come $En(\tau): \exists V' \, \varphi_{\tau}(V,V')$. L'immagine di $\tau$ quindi non è mai l'insieme vuoto.

La **transizione idling** è definita come $\tau_{I}:  V' = V$. La transizione non fa nulla ($s'$ è un successore di $s$ se e solo se hanno lo stesso valore), è sempre abilitata ed è sempre presente nell'insieme delle transizioni. Giustizia e compassione non sono richieste per $\tau_I$. 
Una transizione $\tau \in \mathcal{T}, \tau \neq \tau_{I}$ è detta *diligente*.

## Computazioni di un FTS

Si consideri una sequenza infinita di stati $s_{0}, s_{1},\dots, \, s_{i}\in \Sigma$.
- $\tau \in \mathcal{T}$ è **abilitata** in posizione $k$ della sequenza se $\tau$ è abilitata allo stato $s_k$
- $\tau \in \mathcal{T}$ è **eseguita** in posizione $k$ della sequenza se $s_{k+1} \in \tau(s_k)$ (lo stato $s_{k+1}$ è $\tau$-successore di $s_k$).

Più di una transizione può essere eseguita ad una data posizione.

Una sequenza $\sigma$ è una computazione di un FTS $P$, chiamata anche **$P$-computazione**, se soddisfa:
- **Inizialità**: $s_0$ soddisfa $\theta$, ed è quindi uno stato iniziale.
- **Consequenzialità**: $\forall j = 0,1,2,\dots$, $s_{j+1}$ è un $\tau$-successore di $s_j$ per qualche transizione $\tau$.
- **Giustizia**: per ogni $\tau \in \mathcal{J}$, $\tau$ non è mai abilitata costantemente da una posizione $k$ in avanti e mai eseguita.
- **Compassione**: per ogni $\tau \in \mathcal{J}$, non è possibile che $\tau$ sia abilitata un numero infinito di volte da una posizione $k$ in avanti ma eseguita un numero finito di volte.

### Proprietà delle computazioni
- Uno stato $s$ è **$P$-accessibile** se $s$ occorre in qualche $P$-computazione.
- Se $s$ è il $\tau$-successore di uno stato $P$-accessibile, allora è a sua volta $P$-accessibile.
- Le sequenze $\delta$ che soddisfano i requisiti di inizialità e consequenzialità sono **run**, anche se non rispettano le condizioni di fairness. Se $s$ è $P$-accessibile in una run, è $P$-accessibile in una computazione.


