---
tags: 
  - information-retrieval
---

[[2. Altre metriche#Discounted Cumulative Gain (DCG)|DCG]] è una metrica che da un **ranking totale** su rilevanza **categoriale**.

Si usano categorie discrete (es: 0 = rilevante, 1 = poco rilevante...) e si ordinano i tutti i documenti dal più rilevante al meno rilevante.

I documenti più rilevanti danno più **valore** all'utente, e si vogliono valorizzare i documenti più rilevanti recuperati per primi.

**Cumulative Gain (CG)** somma la rilevanza (categoriale) di tutti i risultati ottenuti da una query.

**Discounted Cumulative Gain** i valori sommati per un fattore relativo alla posizione in cui sono stati recuperati.

$$DCG[i] = \sum_{j=1}^i \frac{G[j]}{1 + \log_b j}$$

dove:

- $G[j]$ è la categoria $j$ (ad esempio $G = \{0, 1, 2, 3\}$)

Le query possono avere un numero diverso di documenti rilevanti: per questo motivo si usa NDCG, normalizzando DCG dividendo i valori per il **valore ideale**.

**sDCG** calcola DCG sui valori DCG delle query di una sessione: il guadagno di una query è scalato in base alla posizione (il momento in cui è stata effettuata) della query.