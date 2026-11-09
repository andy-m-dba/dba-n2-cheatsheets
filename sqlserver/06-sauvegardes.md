# MSSQL-N2-06 — Sauvegardes & restauration

**Quand ?** Job en échec, backup manquant, RPO/PITR douteux.

### Historique msdb (si autorisé)
```sql
SELECT TOP (30) database_name, type,
       backup_start_date, backup_finish_date,
       is_copy_only, first_lsn, last_lsn
FROM msdb.dbo.backupset
ORDER BY backup_finish_date DESC;
```

### Lecture et action
- `D` = full, `I` = différentiel, `L` = journal ; vérifier la chaîne **par base** et les LSN.
- Historique absent ≠ backup inexistant : msdb purgée, sauvegarde externe ou droits limités.
- Un backup enregistré, même réussi, **ne prouve pas** qu'il se restaure.
- **Action N2 :** analyser job, destination, rétention et chaîne ; relance uniquement via procédure approuvée.
- **Validation :** job réussi, objectif RPO et test de restauration sur environnement isolé.

**Escalade N3 :** chaîne incomplète, restauration de production nécessaire ou risque de perte de données. Ne pas restaurer directement en production.

[Sources : backupset](https://learn.microsoft.com/en-us/sql/relational-databases/system-tables/backupset-transact-sql) · [sauvegarde](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
