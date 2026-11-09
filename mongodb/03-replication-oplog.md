# MDB-N2-03 — Réplication & fenêtre oplog

**Quand ?** Secondaire en retard, élection, état `RECOVERING`.

### Sur un membre `mongod` du replica set
```javascript
const s = rs.status()
s.members.map(m => ({
  nom: m.name, etat: m.stateStr, sante: m.health,
  dernierOp: m.optimeDate, source: m.syncSourceHost
}))
```

### Fenêtre d'oplog du nœud (si accès autorisé)
```javascript
rs.printReplicationInfo()
```

### Lecture et action
- `optimeDate` ancien sur tous les nœuds peut simplement traduire **l'absence d'écritures**.
- Comparer les optimes **entre membres**, les heartbeats et la fenêtre oplog.
- `RECOVERING`, membre inaccessible ou fenêtre trop courte : confronter à la supervision.
- `rs.status()` ne s'exécute pas sur `mongos` et peut être indisponible sur Atlas Free/Flex.
- **Action N2 :** qualifier réseau, stockage, charge et source de sync ; correction uniquement sous procédure validée.
- **Validation :** états attendus, optimes cohérents et réplication durable.

**Escalade N3 :** secondaire trop en retard pour l'oplog, réinitialisation ou reconfiguration nécessaire. Aucun `stepDown` autonome.

[Sources : replSetGetStatus](https://www.mongodb.com/docs/manual/reference/command/replsetgetstatus/) · [replication](https://www.mongodb.com/docs/manual/reference/replication/)
