# MSSQL-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident SQL Server persistant après N1.

### 1. Cadrer (2 min)
Heure/fuseau, instance, base, impact, changements récents et alertes N1.

### 2. État des bases et requêtes (3 min)
```sql
SELECT name, state_desc, log_reuse_wait_desc
FROM sys.databases ORDER BY name;

SELECT TOP (15) session_id, blocking_session_id, status,
       wait_type, wait_time AS wait_ms,
       total_elapsed_time AS duree_ms
FROM sys.dm_exec_requests
WHERE session_id <> @@SPID ORDER BY total_elapsed_time DESC;
```

### 3. Orientation (3 min)
[Blocages](01-blocages-transactions.md) · [SQL lent](02-performance-query-store.md) · [Attentes/I/O](03-attentes-io.md) · [tempdb/log](04-tempdb-journal.md) · [Statistiques](05-index-statistiques.md) · [Backups](06-sauvegardes.md) · [Always On](07-always-on.md).

### 4. Décision (2 min)
**Action N2 :** confirmer la cause probable, évaluer l'impact, obtenir l'autorisation, exécuter la procédure approuvée, comparer avant/après et vérifier côté application.

**Escalade N3 :** risque de perte de données, corruption, bascule non planifiée ou diagnostic incertain.

[Source : DMV des requêtes](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql)
