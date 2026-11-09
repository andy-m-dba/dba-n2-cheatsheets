# MSSQL-N2-07 — Always On Availability Groups

**Quand ?** AG dégradé, réplica non connecté, file de synchronisation.

### États des réplicas
```sql
SELECT is_local, role_desc, connected_state_desc,
       synchronization_health_desc
FROM sys.dm_hadr_availability_replica_states;
```

### Bases et files de transport / redo
```sql
SELECT DB_NAME(database_id) AS base, is_local,
       synchronization_state_desc, synchronization_health_desc,
       log_send_queue_size AS send_ko,
       redo_queue_size AS redo_ko, is_suspended
FROM sys.dm_hadr_database_replica_states;
```

### Lecture et action
- Les files sont en **Ko** ; la visibilité et la fraîcheur dépendent du rôle du réplica interrogé.
- `SYNCHRONIZING` peut être normal en commit asynchrone : comparer aux modes attendus.
- `is_suspended=1` ou `NOT_HEALTHY` : examiner réseau, transport, stockage et messages d'erreur.
- **Action N2 :** qualifier l'écart et faire approuver toute reprise de mouvement de données.
- **Validation :** connexion, queues et état par base stabilisés ; test applicatif.

**Escalade N3 :** données potentiellement divergentes, réintégration complexe ou failover. Aucun basculement forcé N2 autonome.

[Sources : AG états](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql) · [files redo](https://learn.microsoft.com/en-us/troubleshoot/sql/database-engine/availability-groups/troubleshooting-recovery-queuing-in-alwayson-availability-group)
