## Problemi su linguaggi regolari

Dati $\mathcal{A}, \mathcal{A'}$ due automi a stati finiti abbiamo:
1. **Emptiness problem**: $L(\mathcal{A}) = \varnothing$
2. **Universality problem**: $L(\mathcal{A}) = A^*$. Può essere ridotto all'emptiness problem: $L(\mathcal{A}) = A^{*} \equiv \lnot L(\mathcal{A}) = \varnothing$
3. **Inclusion problem**: $L(\mathcal{A}) \subseteq L(\mathcal{A'})$. Si riduce all'emptiness problem: possiamo riscrivere $L(\mathcal{A}_1)\subseteq L(\mathcal{A}_2)$ come $L(\mathcal{A}_1) \cap \lnot L(\mathcal{A}_2) =\varnothing$.
4. **Equivalence problem**: $L(\mathcal{A}) = L(\mathcal{A'})$. Si riduce all'inclusion problem: $L(\mathcal{A}_1) = L(\mathcal{A}_2) \equiv L(\mathcal{A}_1) \subseteq L(\mathcal{A}_2) \land L(\mathcal{A}_2) \subseteq L(\mathcal{A}_1)$

### Emptiness problem
#todo 


## Problemi su linguaggi $\omega$-regolari

Dati $\mathcal{A}, \mathcal{A'}$ due automi automi di Büchi abbiamo:

1. **Emptiness problem**: $L(\mathcal{A}) = \varnothing$. Sfruttando le [[Espressioni regolari#$ omega$-parole ultimately periodic|parole UP]] possiamo decidere se un linguaggio $\omega$-regolare è vuoto controllando l'esistenza di parole $UP$. Per fare ciò si cerca all'interno del grafo dell'esecuzione di $\mathcal{A}$ si raggiunge un ciclo: in caso affermativo, una parola $UP$ è stata trovata. Tuttavia non è semplice trasformare una formula in un BA per controllare efficientemente se questa ammette soluzioni. Gli altri problemi sono riducibili all'emptiness problem come per i FSA.
2. **Universality problem**: $L(\mathcal{A}) = A^{\omega}$
3. **Inclusion problem**: $L(\mathcal{A}) \subseteq L(\mathcal{A'})$
4. **Equivalence problem**: $L(\mathcal{A}) = L(\mathcal{A'})$