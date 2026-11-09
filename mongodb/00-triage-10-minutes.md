# MDB-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident MongoDB persistant après le diagnostic N1.

### 1. Cadrer (2 min)
Heure/fuseau, application, cluster et nœud, topologie attendue, impact, derniers changements.

### 2. Vérifier la cible (2 min, mongosh autorisé)
```javascript
db.adminCommand({ ping: 1 })
const h = db.adminCommand({ hello: 1 });
({ replicaSet: h.setName, primaire: h.isWritablePrimary,
   secondaire: h.secondary, mongos: h.msg === "isdbgrid" })
```

### 3. Identifier la charge (3 min, avec droits de supervision)
```javascript
const s = db.serverStatus();
({ connexions: s.connections, files: s.globalLock?.currentQueue })
```
Compteurs dépendants du nœud et de la version ; un `ping` réussi ne prouve pas la disponibilité applicative.

### 4. Orienter (3 min)
[Opérations](01-operations-connexions.md) · [SQL/Index](02-requetes-index.md) · [Replica set](03-replication-oplog.md) · [Stockage](04-stockage-wiredtiger.md) · [Sharding](05-sharding-balancer.md) · [Backups](06-sauvegardes-pitr.md) · [Logs](07-logs-authentification.md).

**Action N2 :** établir les faits, autoriser la correction, mesurer avant/après, confirmer avec l'application.

**Escalade N3 :** risque de perte de données, primaire absent, divergence ou cause non établie.

[Sources : hello](https://www.mongodb.com/docs/manual/reference/command/hello/) · [serverStatus](https://www.mongodb.com/docs/manual/reference/command/serverstatus/)
