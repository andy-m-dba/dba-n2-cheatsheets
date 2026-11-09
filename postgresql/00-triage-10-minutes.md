# PG-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident non résolu après les contrôles N1.

### 1. Cadrer (2 min)
Heure/fuseau, instance et rôle, impact, changements récents, mesures N1, droits N2 autorisés.

### 2. État et connexions (3 min)
```sql
SELECT now(), current_setting('server_version') AS version,
       pg_is_in_recovery() AS standby, pg_postmaster_start_time();
SELECT datname, state, wait_event_type, count(*) AS sessions
FROM pg_stat_activity WHERE backend_type = 'client backend'
GROUP BY datname, state, wait_event_type ORDER BY sessions DESC;
```

### 3. Indicateurs (3 min)
```sql
SELECT datname, deadlocks, temp_bytes, conflicts, stats_reset
FROM pg_stat_database ORDER BY temp_bytes DESC;
```
Compteurs **cumulatifs** : comparer deux mesures horodatées et vérifier les droits de visibilité.

### 4. Orienter (2 min)
[Verrous](01-verrous-transactions.md) · [SQL lent](02-performance-sql.md) · [Vacuum](03-vacuum-statistiques.md) · [WAL](04-capacite-wal-slots.md) · [Réplication](05-replication.md) · [Backups](06-sauvegardes-archivage.md) · [Connexions](07-connexions-ressources.md).

**Action N2 :** diagnostiquer, évaluer l'impact, obtenir l'autorisation, puis valider avant/après. **Escalade N3 :** risque de perte de données, cause incertaine ou intervention dépassant le runbook.

[Source : monitoring](https://www.postgresql.org/docs/18/monitoring-stats.html)
