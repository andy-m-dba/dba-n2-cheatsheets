# MSSQL-N2-02 — Requêtes lentes & Query Store

**Quand ?** Régression SQL, CPU élevé, plan de requête suspect.

### Requêtes actives
```sql
SELECT TOP (15) session_id, status, wait_type, cpu_time,
       logical_reads, total_elapsed_time AS duree_ms
FROM sys.dm_exec_requests
WHERE session_id <> @@SPID
ORDER BY total_elapsed_time DESC;
```

### Query Store — dans la base ciblée, si activé
```sql
SELECT TOP (15) q.query_id, p.plan_id,
       SUM(rs.count_executions) AS executions,
       SUM(rs.avg_duration * rs.count_executions)
           / NULLIF(SUM(rs.count_executions),0) AS moyenne_us
FROM sys.query_store_query AS q
JOIN sys.query_store_plan AS p ON p.query_id = q.query_id
JOIN sys.query_store_runtime_stats AS rs ON rs.plan_id = p.plan_id
GROUP BY q.query_id, p.plan_id
ORDER BY moyenne_us DESC;
```

### Lecture et action
- Query Store doit être activé ; `avg_duration` est en **microsecondes**. Données agrégées sur les intervalles conservés.
- Ne pas identifier une régression sur une moyenne isolée : comparer périodes, exécutions et plans.
- **Action N2 :** envisager optimisation, statistiques ou plan forcing **uniquement après test, validation et rollback documenté**.
- **Validation :** CPU/durée et erreurs applicatives avant/après.

**Escalade N3 :** changement complexe de schéma ou instabilité persistante.

[Source : Query Store](https://learn.microsoft.com/en-us/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)
