# MYSQL-N2-06 — Sauvegardes, binlogs & PITR

**Quand ?** Job échoué, chaîne binlog incomplète, RPO compromis.

### Variables (MySQL et MariaDB)
```sql
SHOW GLOBAL VARIABLES LIKE 'log_bin';
SHOW GLOBAL VARIABLES LIKE 'server_id';
```

### Position du journal courant (privilège adapté)
MySQL 8.4 :
```sql
SHOW BINARY LOG STATUS;
```
MariaDB 10.11 / 11.x :
```sql
SHOW BINLOG STATUS;
```

### Lecture et action
- `log_bin = ON` ne prouve **pas** que les binlogs requis sont conservés et sauvegardés.
- Vérifier le dernier backup réussi, la chaîne full/incr et la continuité binlog dans **l'outil officiel**.
- Un job `success` ne garantit ni cohérence ni restauration ; vérifier les tests isolés.
- **Action N2 :** analyser destination, erreur et rétention ; relance validée uniquement. Ne pas purger de binlogs.
- **Validation :** backup réussi, continuité PITR et objectif RPO vérifiés selon procédure.

**Escalade N3 :** binlogs manquants, reprise incertaine ou restauration en production.

[Sources : MySQL](https://dev.mysql.com/doc/refman/8.4/en/backup-and-recovery.html) · [MariaDB PITR](https://mariadb.com/docs/server/server-usage/backup-and-restore/mariadb-backup/point-in-time-recovery-pitr-mariadb-backup)
