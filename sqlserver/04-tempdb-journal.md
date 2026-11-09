# MSSQL-N2-04 — tempdb & journal de transactions

**Quand ?** `tempdb` ou log saturé, croissance LDF, erreurs d'allocation.

### tempdb — dans n'importe quel contexte autorisé
```sql
SELECT
  SUM(unallocated_extent_page_count)*8.0/1024 AS libre_mo,
  SUM(version_store_reserved_page_count)*8.0/1024 AS versions_mo,
  SUM(internal_object_reserved_page_count)*8.0/1024 AS internes_mo,
  SUM(user_object_reserved_page_count)*8.0/1024 AS objets_mo
FROM tempdb.sys.dm_db_file_space_usage;
```

### Journal — exécuter dans la base ciblée
```sql
SELECT used_log_space_in_percent,
       total_log_size_in_bytes/1048576.0 AS taille_log_mo
FROM sys.dm_db_log_space_usage;
SELECT name, recovery_model_desc, log_reuse_wait_desc
FROM sys.databases WHERE database_id = DB_ID();
```

### Lecture et action
- L'espace interne à `tempdb` ou au log **n'est pas** l'espace libre du volume OS.
- `log_reuse_wait_desc` indique un frein à la réutilisation, **pas** une cause confirmée.
- **Action N2 :** examiner transactions, backups du log, version store et capacité avant correction approuvée.
- **Validation :** occupation et croissance stabilisées ; confirmer l'application.

**Escalade N3 :** saturation imminente, corruption, purge ou modification de recovery model envisagée. Aucun shrink/reset par réflexe.

[Sources : tempdb](https://learn.microsoft.com/en-us/sql/relational-databases/databases/tempdb-database) · [log](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql)
