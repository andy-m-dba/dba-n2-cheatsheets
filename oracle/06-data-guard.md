# ORA-N2-06 — Data Guard : transport & apply

**Quand ?** Standby désynchronisée, erreur de transport, lag anormal.

### Identifier le rôle
```sql
SELECT database_role, open_mode, protection_mode FROM v$database;
```

### Sur la standby uniquement
```sql
SELECT name, value, time_computed, datum_time
FROM v$dataguard_stats
WHERE name IN ('transport lag','apply lag');
```

### Sur la primaire (destinations actives)
```sql
SELECT dest_id, status, error FROM v$archive_dest
WHERE status <> 'INACTIVE' ORDER BY dest_id;
```

### Lecture et action
- `transport lag` : réception du redo ; `apply lag` : application du redo.
- Si `DATUM_TIME` ne change pas entre relevés, la métrique peut être **obsolète** même avec un lag affiché faible.
- Zéro ligne dans `V$DATAGUARD_STATS` sur la primaire : attendu.
- **Action N2 :** qualifier transport, apply, réseau et stockage ; correction/reprise uniquement après validation de la topologie et des risques.
- **Validation :** `DATUM_TIME` progresse, lag revient dans les seuils, cohérence et application confirmées.

**Escalade N3 :** redo manquant, divergence, réinstanciation ou switchover/failover. Aucune bascule autonome N2.

[Sources : diagnostic apply](https://docs.oracle.com/en/database/oracle/oracle-database/19/haovw/redo-apply-troubleshooting-and-tuning.html) · [Vues Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/19/sbydb/oracle-data-guard-in-oracle-database-views.html)
