# CASS-N2-02 — Latence & thread pools

**Quand ?** Timeouts CQL, hausse p99, files d'attente.

### Sur le nœud concerné (JMX autorisé)
```bash
nodetool -h HOST tpstats
nodetool -h HOST proxyhistograms
```

### Pour une table déjà identifiée
```bash
nodetool -h HOST tablehistograms KEYSPACE TABLE
```

### Lecture et action
- `Active` / `Pending` : activité et attente **instantanées** ; comparer plusieurs relevés.
- `Dropped` : messages abandonnés, **compteur cumulatif** ; mesurer la variation.
- `proxyhistograms` : percentiles côté coordinateur, **pas** latence réseau de bout en bout.
- `tablehistograms` : latence locale et distribution des partitions pour la table ciblée.
- **Action N2 :** corréler pool, table, driver, CPU/I/O et charge ; tuning uniquement après validation et plan de retour.
- **Validation :** p95/p99 et erreurs stabilisées sur une période comparable.

**Escalade N3 :** saturation persistante, déséquilibre majeur ou modification des pools/timeouts risquée.

[Sources : tpstats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/tpstats.html) · [proxyhistograms](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/proxyhistograms.html)
