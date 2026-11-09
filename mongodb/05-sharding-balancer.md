# MDB-N2-05 — Sharding & balancer

**Quand ?** Shard inaccessible, répartition déséquilibrée, migrations en échec.

### Uniquement connecté à `mongos` (droits autorisés)
```javascript
db.adminCommand({ hello: 1 }).msg   // attendu : "isdbgrid"
db.adminCommand({ listShards: 1 }).shards.map(s => s._id)
sh.getBalancerState()
sh.isBalancerRunning()
```

### Lecture et action
- `listShards` retourne les shards **configurés**, pas leur état de santé.
- `getBalancerState()` indique si le balancer est **autorisé**, pas s'il travaille.
- `isBalancerRunning()` indique une activité de balancer, pas son succès.
- Examiner logs, métriques des shards et migrations avant de conclure.
- **Action N2 :** identifier le shard/routeur concerné ; toute modification du balancer ou de placement exige un accord et un plan de retour.
- **Validation :** requêtes, migrations et disponibilité conformes à la topologie attendue.

**Escalade N3 :** config server dégradé, chunks bloqués, changement de shard key ou intervention de topologie.

[Sources : listShards](https://www.mongodb.com/docs/manual/reference/command/listshards/) · [Balancer](https://www.mongodb.com/docs/manual/tutorial/manage-sharded-cluster-balancer/)
