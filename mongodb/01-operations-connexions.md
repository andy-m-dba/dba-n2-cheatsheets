# MDB-N2-01 — Opérations longues & connexions

**Quand ?** Timeouts, pool saturé ou opérations longues.

### Connexions au nœud
```javascript
const c = db.serverStatus().connections;
({ actuelles: c.current, actives: c.active,
   disponibles: c.available, creees: c.totalCreated })
```

### Opérations longues — base admin, privilège `inprog`
```javascript
db.getSiblingDB("admin").aggregate([
  { $currentOp: { allUsers: true, idleConnections: false } },
  { $match: { active: true, secs_running: { $gte: 5 } } },
  { $project: { opid: 1, op: 1, ns: 1, secs_running: 1,
                waitingForLock: 1, shard: 1 } },
  { $limit: 15 }
]).toArray()
```

### Lecture et action
- `current` = connexions entrantes ; `totalCreated` est cumulatif. Comparer deux relevés.
- 5 secondes = **filtre de recherche**, pas seuil universel d'incident.
- Sur `mongos`, `$currentOp` peut agréger les shards. Sans `inprog`, visibilité restreinte/refusée.
- **Action N2 :** identifier client, durée, charge et pool ; correction après accord. Ne pas lancer `killOp` par défaut.
- **Validation :** timeouts, connexions et opérations stabilisés, application confirmée.

**Escalade N3 :** transaction distribuée bloquée, interruption risquée ou surcharge persistante.

[Sources : $currentOp](https://www.mongodb.com/docs/manual/reference/operator/aggregation/currentOp/) · [serverStatus](https://www.mongodb.com/docs/manual/reference/command/serverstatus/)
