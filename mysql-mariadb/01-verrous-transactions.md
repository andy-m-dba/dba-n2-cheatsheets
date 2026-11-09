# MYSQL-N2-01 — Verrous & transactions

**Quand ?** Timeout, `Lock wait timeout exceeded`, transactions bloquées.

### MySQL 8.x — si schéma sys disponible
```sql
SELECT waiting_pid, blocking_pid, wait_age
FROM sys.innodb_lock_waits
ORDER BY wait_age DESC LIMIT 15;
```

### MariaDB 10.11 / 11.x — privilège PROCESS
```sql
SELECT r.trx_mysql_thread_id AS attente,
       b.trx_mysql_thread_id AS bloqueur,
       r.trx_started AS debut_attente, b.trx_started AS debut_bloqueur
FROM information_schema.INNODB_LOCK_WAITS AS w
JOIN information_schema.INNODB_TRX AS r
  ON r.trx_id = w.requesting_trx_id
JOIN information_schema.INNODB_TRX AS b
  ON b.trx_id = w.blocking_trx_id
ORDER BY r.trx_started LIMIT 15;
```

### Lecture et action
- Un bloqueur peut être inactif ; confirmer la transaction, l'application et le propriétaire.
- Résultat vide = aucun verrou InnoDB visible **à cet instant**.
- **Action N2 :** envisager annulation/interruption uniquement après autorisation : risque de rollback et d'erreurs métier.
- **Validation :** attente disparue, service rétabli et transactions stabilisées.

**Escalade N3 :** transaction critique, deadlocks récurrents ou interruption risquée.

[Sources : MySQL](https://dev.mysql.com/doc/refman/8.4/en/innodb-information-schema-examples.html) · [MariaDB](https://mariadb.com/docs/server/reference/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_lock_waits-table)
