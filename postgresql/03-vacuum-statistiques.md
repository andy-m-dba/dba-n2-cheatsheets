# PG-N2-03 — Autovacuum & statistiques

**Quand ?** Tables en croissance, statistiques anciennes, risque XID.

### Tables suspectes (base courante)
```sql
SELECT schemaname, relname, n_live_tup, n_dead_tup,
       last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables ORDER BY n_dead_tup DESC LIMIT 15;
```

### VACUUM en cours et âge XID
```sql
SELECT pid, datname, phase, heap_blks_scanned, heap_blks_total
FROM pg_stat_progress_vacuum;
SELECT datname, age(datfrozenxid) AS age_xid
FROM pg_database ORDER BY age_xid DESC;
```

### Lecture et action
- `n_dead_tup` est **estimé** ; `last_autovacuum NULL` ne prouve pas une panne.
- La progression du VACUUM n'est pas une estimation fiable du temps restant.
- **Action N2 autorisée :** envisager `VACUUM (ANALYZE)` ciblé après analyse I/O, charge et verrous ; opération coûteuse, sans rollback opérationnel simple.
- **Validation :** évolution des statistiques et des performances, pas le seul nombre de lignes mortes.

**Escalade N3 :** menace de wraparound, VACUUM bloqué ou maintenance risquée.

[Sources : statistiques](https://www.postgresql.org/docs/18/monitoring-stats.html) · [VACUUM](https://www.postgresql.org/docs/18/sql-vacuum.html)
