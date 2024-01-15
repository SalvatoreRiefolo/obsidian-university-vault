---
tags: 
  - automated-reasoning
---

1. Definizione formale CSP, COP e soluzione.
	- Come si può usare CSP per risolvere COP
	- Decidere se un CSP su domini finiti è consistente è NP-Hard?
	- Che scelte non deterministiche vengono fatte durante la constraint based search per CSP?
	- Soluzione in $P$ dati predicati di vincoli di confronto e domini espliciti?
	- Definire SAT, riduzione a CSP con domini $\{0,1\}$.
1. Definizione constraint propagation e sua complessità.
	- Nozione precisa di **arc consistency** e come si può raggiungere per congiunzione di vincoli binari.
	- Nozione precisa di **hyper arc consistency** e come si può raggiungere per congiunzione di vincoli binari.
	- Come ottenere hyper arc consistency per **AllDifferent** e la sua complessità
	- **Path consistency**, come si raggiunge e complessità.
	- **Unit propagation** e relazione con arc consistency.
1. Usare Local Search per risolvere COP, combinare constraint based e local search.
2. Problema Minizinc da risolvere e generalizzare.
	- Spiegare cosa fa COP e dare soluzioni a CSP corrispondente.
	- Cos'è Minizinc, come funziona.
1. Definizione formale sintassi programmi:
	- Generali
	- Termine, sostituzione, unifier, most general unifier, SLD resolution step.
	- Esempio dove risoluzione SLD entra in loop.
	- Program completion *comp(P)*
	- Riscrivere programma usando sintassi di base dei programmi generali.
2. Dato programma P definire operatore $T_p$ e dimostrare che è:
	- **Monotono** per programmi definiti
	- **Non monotono** per programmi generali.
	- Si definisca $T_p \uparrow \omega$ e cosa permette di calcolare
3. Calcolare i modelli stabili dato un programma proposizionale.
4. Se P è definito e M_1 e M_2 sono modelli di Herbrand, M_1 \cap M_2 è un modello di Herbrand. Mostrare che non vale con programmi generali.
1. Definire la nozione di modello stabile e dimostrare che decidere se P ammette un modello stabile è NP-completo:
	- Programmi logici disgiuntivi senza negazione
	- Programmi ground generali
	- Programmi definiti
2. Descrivere aggiornamento di uno stato che ha a che fare con tutti i fluent e possibilmente altri fluent quando l'azione $a$ è applicata. 
	- Complessità ADL