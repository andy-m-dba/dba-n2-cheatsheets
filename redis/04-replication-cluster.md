# REDIS-N2-04 — Réplication & Redis Cluster

**Quand ?** Replica déconnecté, failover, slots indisponibles ou resynchronisation.

### Sur chaque nœud concerné
```text
INFO replication
```
En mode **Redis Cluster** uniquement :
```text
CLUSTER INFO
```

### Lecture et action
- Sur replica, `master_link_status:down` et `master_last_io_seconds_ago` : vérifier réseau et source.
- `master_repl_offset` / `slave_repl_offset` : positions **en octets**, pas un retard en secondes.
- `repl_backlog_histlen` : volume conservé pour resynchronisation partielle ; ne garantit pas sa réussite.
- Cluster : `cluster_slots_assigned` attendu à **16384** ; `cluster_slots_fail > 0` ou `cluster_state:fail` : escalade.
- **Action N2 :** comparer rôle, offsets, slots et supervision ; toute reprise/failover nécessite procédure approuvée.
- **Validation :** liens actifs, progression des offsets, slots couverts et test applicatif.

**Escalade N3 :** resynchronisation complète risquée, split-brain suspecté ou bascule complexe.

[Sources : réplication](https://redis.io/docs/latest/manual/replication/) · [CLUSTER INFO](https://redis.io/docs/latest/commands/cluster-info/)
