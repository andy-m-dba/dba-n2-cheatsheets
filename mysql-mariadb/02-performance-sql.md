# MYSQL-N2-02 — SQL lent & plans

**Quand ?** Régression SQL, CPU élevé, requête suspecte.

### Instrumentation disponible ?
```sql
SHOW GLOBAL VARIABLES LIKE 'performance_schema';
```

### Digests agrégés (si Performance Schema actif)
```sql
SELECT SCHEMA_NAME, DIGEST, COUNT_STAR,
       ROUND(SUM_TIMER_WAIT/1e12,2) AS total_s,
       ROUND(AVG_TIMER_WAIT/1e9,2) AS moyenne_ms
FROM performance_schema.events_statements_summary_by_digest
WHERE DIGEST IS NOT NULL
ORDER BY SUM_TIMER_WAIT DESC LIMIT 15;
```

### Plan estimé d'une requête ciblée
```sql
EXPLAIN SELECT * FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'mysql';
```

### Lecture et action
- `*_TIMER_WAIT` est en **picosecondes** ; statistiques cumulatives, pas mesures instantanées.
- MariaDB désactive souvent Performance Schema par défaut : **ne pas l'activer pendant l'incident** sans procédure.
- `EXPLAIN` affiche un plan ; `EXPLAIN ANALYZE` exécute la requête et peut créer de la charge.
- **Action N2 :** proposer index, statistiques ou réécriture après comparaison des plans, test et rollback prévu.

**Escalade N3 :** régression majeure ou modification de schéma à fort impact.

[Sources : MySQL](https://dev.mysql.com/doc/refman/8.4/en/performance-schema-statement-summary-tables.html) · [MariaDB](https://mariadb.com/docs/server/reference/system-tables/performance-schema/performance-schema-overview)
