# ORA-N2-02 — SQL lent & performance

**Quand ?** Temps SQL en hausse, CPU/I/O élevés, plan suspect.

### Activité SQL en cache (lecture seule)
```sql
SELECT sql_id, plan_hash_value, executions,
       ROUND(elapsed_time/1000000,1) AS total_s,
       ROUND(cpu_time/1000000,1) AS cpu_s,
       last_active_time
FROM v$sqlstats
WHERE executions > 0
ORDER BY elapsed_time DESC FETCH FIRST 15 ROWS ONLY;
```

### Sessions actives (sans texte SQL)
```sql
SELECT sid, serial#, sql_id, event, wait_class
FROM v$session
WHERE status = 'ACTIVE' AND type = 'USER'
FETCH FIRST 20 ROWS ONLY;
```

### Lecture et action
- `ELAPSED_TIME` / `CPU_TIME` sont en **microsecondes cumulées** ; comparer deux relevés et la charge.
- `PLAN_HASH_VALUE` aide à repérer un changement de plan, pas à prouver la régression.
- **AWR/ASH/ADDM nécessitent Oracle Diagnostics Pack** : ne pas utiliser sans licence validée.
- **Action N2 :** corréler SQL ID, plan et évolution ; tuning/statistiques selon procédure avec évaluation d'impact et retour arrière.
- **Validation :** délais et charge réels avant/après, confirmation applicative.

**Escalade N3 :** régression persistante, plan instable ou modification de schéma lourde.

[Sources : V$SQLSTATS](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-SQLSTATS.html) · [Licences](https://docs.oracle.com/en/database/oracle/oracle-database/19/dblic/Licensing-Information.html)
