# CASS-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident Cassandra persistant après N1.

### 1. Cadrer (2 min)
Heure/fuseau, cluster/DC, nœud, impact, messages expurgés, changements et mesures N1.

### 2. Vérifier le cluster (3 min, accès JMX autorisé)
```bash
nodetool -h HOST status
nodetool -h HOST info
```
`nodetool` utilise JMX, distinct du port CQL. Adapter JMX, TLS et droits à l'inventaire.

### 3. Identifier la charge (3 min)
```bash
nodetool -h HOST tpstats
nodetool -h HOST compactionstats
```
`Pending` est instantané ; `Dropped` est cumulatif. Comparer plusieurs relevés.

### 4. Orienter (2 min)
[Cluster](01-cluster-gossip.md) · [Latence](02-latence-threadpools.md) · [Stockage](03-stockage-tables.md) · [Compactions](04-compactions-tombstones.md) · [Cohérence](05-repair-streaming.md) · [Backups](06-sauvegardes-snapshots.md) · [Clients](07-clients-cql-logs.md).

**Action N2 :** établir les faits, évaluer le risque, obtenir l'autorisation, intervenir via la procédure validée, comparer avant/après.

**Escalade N3 :** perte de données possible, cluster dégradé critique, réparation complexe ou cause non établie.

[Sources : status](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/status.html) · [tpstats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/tpstats.html)
