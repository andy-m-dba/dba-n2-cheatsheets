# MSSQL-N2-01 — Blocages & transactions

**Quand ?** Timeout, chaîne de blocage, sessions bloquées.

### Identifier les sessions en attente
```sql
SELECT r.session_id, r.blocking_session_id, r.wait_type,
       r.wait_time AS wait_ms, r.total_elapsed_time AS elapsed_ms,
       s.login_time, s.status AS session_status
FROM sys.dm_exec_requests AS r
LEFT JOIN sys.dm_exec_sessions AS s ON s.session_id = r.session_id
WHERE r.blocking_session_id <> 0
ORDER BY r.wait_time DESC;
```

### Identifier les transactions ouvertes
```sql
SELECT st.session_id, at.transaction_begin_time,
       at.name AS transaction_name
FROM sys.dm_tran_session_transactions AS st
JOIN sys.dm_tran_active_transactions AS at
  ON at.transaction_id = st.transaction_id
ORDER BY at.transaction_begin_time;
```

### Lecture et action
- `blocking_session_id > 0` : SPID bloquant ; `-2` à `-5` : états spéciaux, pas des SPID ordinaires.
- Un bloqueur peut être **inactif** et ne pas apparaître dans `sys.dm_exec_requests`.
- **Action N2 autorisée :** identifier application, propriétaire, transaction et impact avant d'envisager `KILL`. Celui-ci peut provoquer un rollback long et des erreurs applicatives.
- **Validation :** requêtes débloquées, transactions stabilisées et confirmation applicative.

**Escalade N3 :** transaction distribuée/orpheline ou interruption à fort impact.

[Source : diagnostic des blocages](https://learn.microsoft.com/en-us/troubleshoot/sql/database-engine/performance/understand-resolve-blocking)
