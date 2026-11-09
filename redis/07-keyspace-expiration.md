# REDIS-N2-07 — Keyspace, expirations & cache

**Quand ?** Taux de cache hit en baisse, expirations inattendues, croissance des clés.

### Statistiques du nœud
```text
INFO keyspace
INFO stats
```

### Lecture et action
- `db0` / `dbN` : nombre de clés et de clés avec TTL, plus TTL moyen **estimé** ; valeurs propres au nœud.
- `keyspace_hits` / `keyspace_misses` : accès réussis/échoués, **compteurs cumulés**.
- Calculer le hit ratio sur **deux intervalles** : Δhits / (Δhits + Δmisses), si dénominateur non nul.
- `expired_keys` : expirations constatées ; elles ne surviennent pas toujours exactement à l'échéance du TTL.
- **Action N2 :** comparer stratégie de cache, TTL et charge avec l'application ; changement d'expiration uniquement après validation.
- **Validation :** hit ratio, mémoire et erreurs métier sur période comparable, sans supprimer de clés.

**Escalade N3 :** perte de clés persistantes, incohérence multi-nœuds ou refonte du cache nécessaire.

[Sources : INFO](https://redis.io/docs/latest/commands/info/) · [Évictions/cache](https://redis.io/docs/latest/reference/eviction/)
