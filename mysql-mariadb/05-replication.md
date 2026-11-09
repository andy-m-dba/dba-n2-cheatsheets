# MYSQL-N2-05 — Réplication & retard

**Quand ?** Replica déconnecté, erreur de thread, lag ou GTID incohérent.

### MySQL 8.0.22+ / 8.4 — sur le réplica
```sql
SHOW REPLICA STATUS\G
```
Pour MySQL 8.0 antérieur à 8.0.22 : `SHOW SLAVE STATUS\G`.

### MariaDB 10.11 / 11.x — tous les canaux
```sql
SHOW ALL SLAVES STATUS\G
```

### Lecture et action
- MySQL : `Replica_IO_Running`, `Replica_SQL_Running`, `Seconds_Behind_Source`.
- MariaDB : `Slave_IO_Running`, `Slave_SQL_Running`, `Seconds_Behind_Master`.
- Un retard `0` **ne garantit pas** un réplica à jour, notamment si le récepteur est lui-même en retard.
- Examiner `Last_IO_Error` / `Last_SQL_Error` expurgés, canal attendu et GTID/positions.
- **Action N2 :** qualifier réseau, source, stockage et erreur ; reprise uniquement sur procédure approuvée.
- **Validation :** threads actifs, erreurs disparues et réplication cohérente sur plusieurs relevés.

**Escalade N3 :** GTID manquants/divergents, réinitialisation ou bascule requise.

[Sources : MySQL](https://dev.mysql.com/doc/refman/8.4/en/show-replica-status.html) · [MariaDB](https://mariadb.com/docs/server/reference/sql-statements/administrative-sql-statements/show/show-replica-status)
