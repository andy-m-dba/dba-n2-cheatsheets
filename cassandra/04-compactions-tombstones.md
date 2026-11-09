# CASS-N2-04 — Compactions & tombstones

**Quand ?** Compactions en retard, disque/CPU sous pression, lectures dégradées.

### Sur le nœud ciblé (JMX autorisé)
```bash
nodetool -h HOST compactionstats
nodetool -h HOST getcompactionthroughput
nodetool -h HOST tablestats KEYSPACE.TABLE
```

### Lecture et action
- `compactionstats` : tâches actives/en attente ; progression et temps restant **estimés**.
- `getcompactionthroughput` : plafond de débit de compaction configuré, pas débit réellement observé.
- `tablestats` : SSTables et indicateurs de tombstones sur une **table ciblée**.
- Des tombstones ne sont pas automatiquement anormaux ; corréler TTL, suppressions, stratégie et latence.
- **Action N2 :** revoir charge et stratégie, proposer réglage après test/validation ; ne pas lancer de compaction forcée réflexe.
- **Validation :** file de compaction, latence, I/O et espace suivis sur plusieurs mesures.

**Escalade N3 :** tombstones massifs, stratégie à modifier ou risque autour de `gc_grace_seconds`. Ne pas réduire la grâce sans analyse de cohérence.

[Sources : compactionstats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/compactionstats.html) · [compaction](https://cassandra.apache.org/doc/stable/cassandra/managing/operating/compaction/overview.html)
