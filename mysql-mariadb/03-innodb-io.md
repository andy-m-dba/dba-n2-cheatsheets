# MYSQL-N2-03 — InnoDB, cache & I/O

**Quand ?** Latence disque, contention InnoDB, hausse des lectures physiques.

### Compteurs InnoDB
```sql
SHOW GLOBAL STATUS WHERE Variable_name IN
 ('Innodb_buffer_pool_read_requests','Innodb_buffer_pool_reads',
  'Innodb_row_lock_waits','Innodb_row_lock_time');
SHOW GLOBAL VARIABLES LIKE 'innodb_buffer_pool_size';
```

### Diagnostic ponctuel (privilège PROCESS)
```sql
SHOW ENGINE INNODB STATUS\G
```

### Lecture et action
- `Innodb_buffer_pool_reads` : lectures physiques ; comparer son **évolution** aux requêtes logiques.
- `Innodb_row_lock_waits` et `Innodb_row_lock_time` : statistiques cumulatives de contention.
- `SHOW ENGINE INNODB STATUS` peut contenir du SQL métier : **expurger** avant partage.
- Une hausse I/O ne prouve pas un buffer pool trop petit ; corréler disque, requêtes et charge.
- **Action N2 :** proposer réglage ou optimisation seulement après mesure, autorisation et plan de retour.

**Escalade N3 :** panne stockage, corruption suspectée ou tuning structurel complexe.

[Sources : MySQL InnoDB](https://dev.mysql.com/doc/refman/8.4/en/innodb-monitors.html) · [MariaDB](https://mariadb.com/docs/server/reference/sql-statements/administrative-sql-statements/show/show-engine-innodb-status)
