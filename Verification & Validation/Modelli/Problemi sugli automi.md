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
1. **Universality problem**: $L(\mathcal{A}) = A^{\omega}$
2. **Inclusion problem**: $L(\mathcal{A}) \subseteq L(\mathcal{A'})$
3. **Equivalence problem**: $L(\mathcal{A}) = L(\mathcal{A'})$

Dimostrabili come per gli automi a stati finiti.

### Chiusura sotto complementazione