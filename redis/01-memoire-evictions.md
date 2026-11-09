# REDIS-N2-01 — Mémoire, OOM & évictions

**Quand ?** `OOM`, hausse mémoire, évictions ou limite `maxmemory` atteinte.

### Sur le nœud concerné
```text
INFO memory
INFO stats
MEMORY STATS
```

### Lecture et action
- `used_memory` : allocations Redis ; `used_memory_rss` : mémoire résidente du processus.
- `maxmemory` : limite Redis ; `maxmemory_policy` : stratégie d'éviction.
- `evicted_keys` : compteur **cumulatif**. Mesurer sa variation, avec l'impact applicatif.
- `used_memory_rss` supérieur à `used_memory` ne prouve pas à lui seul une fuite mémoire.
- **Action N2 :** identifier croissance, stratégie et capacité par nœud ; proposer ajustement après mesure, autorisation et retour arrière documenté.
- **Validation :** mémoire stable, évictions/erreurs et latence dans les seuils convenus.

**Escalade N3 :** OOM persistant, saturation système ou risque de perte de clés. Aucune purge automatique.

[Sources : INFO](https://redis.io/docs/latest/commands/info/) · [MEMORY STATS](https://redis.io/docs/latest/commands/memory-stats/)
