# REDIS-N2-03 — Connexions & buffers clients

**Quand ?** Connexions rejetées, clients bloqués ou mémoire de buffers élevée.

### Indicateurs du nœud
```text
INFO clients
INFO stats
```

### Détail ponctuel, seulement si autorisé
```text
CLIENT LIST TYPE NORMAL
```

### Lecture et action
- `connected_clients` : connexions actuelles ; confronter à `maxclients` et aux pools.
- `rejected_connections` : connexions refusées **à cause de maxclients** ; compteur cumulatif.
- `blocked_clients` : commandes bloquantes en attente, potentiellement normales.
- `CLIENT LIST` : `age`, `idle`, `qbuf`, `omem` ; sortie sensible (IP, identités), potentiellement volumineuse.
- **Action N2 :** isoler une fuite de connexions ou pression de buffers ; ajustement de pool/limite seulement avec autorisation et rollback.
- **Validation :** baisse des rejets et buffers, stabilité des connexions et du service.

**Escalade N3 :** épuisement persistant, intervention massive sur clients ou architecture à modifier.

[Sources : INFO](https://redis.io/docs/latest/commands/info/) · [CLIENT LIST](https://redis.io/docs/latest/commands/client-list/)
