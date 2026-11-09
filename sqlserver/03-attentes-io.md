# MSSQL-N2-03 — Attentes & I/O

**Quand ?** Latence anormale, CPU/I/O suspectés.

### Attentes SQL Server
```sql
SELECT TOP (15) wait_type, waiting_tasks_count,
       wait_time_ms, signal_wait_time_ms
FROM sys.dm_os_wait_stats
ORDER BY wait_time_ms DESC;
```

### I/O par fichier
```sql
SELECT DB_NAME(v.database_id) AS base, m.type_desc,
       v.num_of_reads, v.io_stall_read_ms,
       v.num_of_writes, v.io_stall_write_ms
FROM sys.dm_io_virtual_file_stats(NULL,NULL) AS v
JOIN sys.master_files AS m
  ON m.database_id=v.database_id AND m.file_id=v.file_id
ORDER BY v.io_stall_read_ms+v.io_stall_write_ms DESC;
```

### Lecture et action
- `wait_time_ms` et `io_stall_*` sont **cumulatifs** ; calculer des écarts sur deux mesures horodatées.
- Une attente élevée ne prouve pas un disque lent ; corréler CPU, stockage, requêtes et supervision.
- **Action N2 :** identifier le fichier et la charge, puis faire valider toute modification de stockage/paramètre.
- **Validation :** latence, attentes et impact métier améliorés sur une période comparable.

**Escalade N3 :** panne stockage, forte régression ou incident d'architecture.

[Sources : waits](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) · [I/O](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql)
