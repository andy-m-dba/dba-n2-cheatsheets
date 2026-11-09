# PG-N2-02 — Requêtes lentes & plans

**Quand ?** Régression de temps SQL, CPU ou I/O élevés.

### Requêtes actives
```sql
SELECT pid, datname, wait_event_type, wait_event,
       now() - query_start AS duree
FROM pg_stat_activity WHERE state = 'active'
ORDER BY query_start NULLS LAST LIMIT 15;
```

### Si pg_stat_statements est déjà installé
```sql
SELECT queryid, calls, round(total_exec_time::numeric,1) AS total_ms,
       round(mean_exec_time::numeric,1) AS moyenne_ms
FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 15;
```

### Plan estimé (exemple sans exécution)
```sql
EXPLAIN (COSTS ON)
SELECT * FROM pg_catalog.pg_class WHERE oid = 'pg_class'::regclass;
```

### Lecture et action
- `pg_stat_statements` est optionnel ; ne pas modifier `shared_preload_libraries` durant l'incident sans procédure.
- Temps et appels sont cumulatifs : comparer par intervalle.
- `EXPLAIN` estime ; **`EXPLAIN ANALYZE` exécute la requête** et peut modifier les données si la requête écrit.
- **Action N2 :** corréler requête, plan et déploiement. Index/tuning uniquement après validation de l'impact et du retour arrière.

**Escalade N3 :** régression lourde ou modification de schéma complexe.

[Sources : pg_stat_statements](https://www.postgresql.org/docs/18/pgstatstatements.html) · [EXPLAIN](https://www.postgresql.org/docs/18/sql-explain.html)
