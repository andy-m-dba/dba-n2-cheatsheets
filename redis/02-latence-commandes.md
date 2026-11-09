# REDIS-N2-02 — Latence & commandes coûteuses

**Quand ?** Temps de réponse élevé, pics CPU, commandes suspectes.

### Métriques sur le nœud ciblé
```text
INFO latencystats
INFO commandstats
INFO cpu
```

### Si autorisé : événements et slow log
```text
LATENCY LATEST
SLOWLOG GET 5
```

### Lecture et action
- `latencystats` : percentiles d'exécution des **commandes**, pas de latence réseau.
- `commandstats` : nombre d'appels et temps CPU cumulé par commande ; comparer deux mesures.
- `LATENCY LATEST` : événements enregistrés **si le moniteur est activé** ; sortie vide ≠ absence de pics.
- `SLOWLOG GET` : commandes lentes selon seuil configuré ; peut révéler arguments, clés et adresses. Ne pas copier la sortie brute.
- **Action N2 :** corréler charge, commandes et métriques applicatives ; réglage ou optimisation uniquement après validation.
- **Validation :** p95/p99 et charge sur une période comparable, sans régression applicative.

**Escalade N3 :** pics persistants, commandes structurantes ou intervention risquée.

[Sources : INFO](https://redis.io/docs/latest/commands/info/) · [SLOWLOG](https://redis.io/docs/latest/commands/slowlog-get/) · [LATENCY](https://redis.io/docs/latest/commands/latency-latest/)
