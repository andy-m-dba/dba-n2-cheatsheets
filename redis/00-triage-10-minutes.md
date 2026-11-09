# REDIS-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident Redis non résolu après les contrôles N1.

### 1. Cadrer (2 min)
Heure/fuseau, application, nœud/cluster, topologie attendue, impact et changements récents.

### 2. Connexion autorisée (2 min)
```bash
redis-cli -h HOST -p PORT --tls --user LECTURE --askpass
```
Adapter TLS et certificats à l'inventaire. Mot de passe à l'invite, jamais en argument.

### 3. Identifier la piste (3 min, dans redis-cli)
```text
PING
INFO server
INFO clients
INFO memory
INFO stats
INFO replication
```
`PONG` confirme la réponse du nœud, **pas** la disponibilité du service complet. `INFO` dépend des ACL.

### 4. Orienter (3 min)
[Mémoire](01-memoire-evictions.md) · [Latence](02-latence-commandes.md) · [Clients](03-connexions-clients.md) · [Réplication](04-replication-cluster.md) · [Persistance](05-persistance-backups.md) · [ACL](06-acl-authentification.md) · [Keyspace](07-keyspace-expiration.md).

**Action N2 :** confirmer l'écart, évaluer l'impact, faire autoriser la correction, vérifier avant/après avec l'application.

**Escalade N3 :** perte de données possible, cluster indisponible, failover complexe ou cause indéterminée.

[Source : INFO](https://redis.io/docs/latest/commands/info/)
