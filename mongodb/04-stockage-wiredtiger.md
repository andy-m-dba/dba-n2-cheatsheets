# MDB-N2-04 — Capacité & WiredTiger

**Quand ?** Disque presque plein, cache sous pression, données en croissance.

### Taille de la base ciblée
```javascript
db.getSiblingDB("DATABASE").runCommand({
  dbStats: 1, scale: 1024 * 1024
})
```

### Cache du nœud WiredTiger, si accessible
```javascript
const c = db.serverStatus().wiredTiger?.cache;
({ utilise: c?.["bytes currently in the cache"],
   maximum: c?.["maximum bytes configured"] })
```

### Volumes OS (self-managed, hôte autorisé)
```bash
df -h
df -i
```

### Lecture et action
- `dataSize` = taille logique ; `storageSize` = espace alloué aux collections, pas disque libre.
- Cache WiredTiger ≠ mémoire totale du processus. En cluster sharded, examiner **chaque shard**.
- `dbStats` et métriques cache peuvent être limités par les droits ou l'offre Atlas.
- **Action N2 :** identifier pression, croissance et volume réel ; extension/maintenance uniquement après validation.
- **Validation :** espace libre, I/O et latence stabilisés sur plusieurs mesures.

**Escalade N3 :** saturation imminente, compactage risqué ou problème de stockage structurel. Aucune purge manuelle.

[Sources : dbStats](https://www.mongodb.com/docs/manual/reference/command/dbstats/) · [serverStatus](https://www.mongodb.com/docs/manual/reference/command/serverstatus/)
