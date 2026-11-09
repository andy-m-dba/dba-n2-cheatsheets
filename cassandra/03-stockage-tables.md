# CASS-N2-03 — Stockage & SSTables

**Quand ?** Disque critique, croissance anormale ou écart de capacité entre nœuds.

### Volume de la table ciblée (JMX autorisé)
```bash
nodetool -h HOST tablestats KEYSPACE.TABLE
nodetool -h HOST status
```

### Filesystems (hôte autorisé)
```bash
df -h
df -i
```

### Lecture et action
- `Space used (live)` / `Space used (total)` : volumes SSTables ; **pas** l'espace libre OS.
- `SSTable count` : indicateur à corréler à la stratégie de compaction, pas seuil de panne.
- `Load` de `status` : données sur le nœud, **pas** taux d'occupation disque.
- Inclure commitlogs, snapshots, temporaires et volumes de **chaque nœud** dans le diagnostic.
- **Action N2 :** confirmer la source de croissance ; extension ou correction uniquement avec analyse de capacité et autorisation.
- **Validation :** occupation et tendance stabilisées, service opérationnel.

**Escalade N3 :** saturation imminente, corruption ou nettoyage destructif envisagé. Aucun `cleanup` ou effacement de SSTables sans plan validé.

[Sources : tablestats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/tablestats.html) · [status](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/status.html)
