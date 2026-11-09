# ORA-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident Oracle non résolu par le N1.

### 1. Cadrer (2 min)
Heure/fuseau, service et PDB, instance/RAC, rôle Data Guard, impact, erreur ORA et derniers changements.

### 2. État de l'instance (2 min, droits autorisés)
```sql
SELECT instance_name, status, database_status, startup_time
FROM v$instance;
SELECT name, open_mode, database_role, log_mode FROM v$database;
```

### 3. Erreurs récentes (3 min)
```sql
SELECT originating_timestamp, message_text
FROM v$diag_alert_ext
WHERE originating_timestamp >= SYSTIMESTAMP - INTERVAL '1' HOUR
  AND message_type IN (2, 3, 4)
ORDER BY originating_timestamp DESC FETCH FIRST 15 ROWS ONLY;
```
Les messages peuvent contenir des données sensibles : **expurger**. Une erreur proche dans le temps n'est pas une cause confirmée.

### 4. Orienter (3 min)
[Verrous](01-sessions-verrous.md) · [Performance](02-performance-sql.md) · [Espace/FRA](03-tablespaces-fra.md) · [Undo](04-undo.md) · [RMAN](05-rman-archivage.md) · [Data Guard](06-data-guard.md) · [CDB/PDB](07-cdb-pdb.md).

**Action N2 :** confirmer les faits, évaluer l'impact, obtenir l'autorisation, appliquer la procédure approuvée, comparer avant/après.

**Escalade N3 :** risque de perte de données, corruption, indisponibilité critique ou diagnostic indéterminé.

[Sources : V$INSTANCE](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-INSTANCE.html) · [V$DIAG_ALERT_EXT](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-DIAG_ALERT_EXT.html)
