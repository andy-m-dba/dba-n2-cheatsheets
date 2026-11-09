# PG-N2-01 — Verrous & transactions longues

**Quand ?** Timeout, requête bloquée, transaction persistante.

### Attentes de verrou
```sql
WITH waiting AS MATERIALIZED (
  SELECT pid, datname, usename,
         now() - xact_start AS age_transaction,
         pg_blocking_pids(pid) AS bloqueurs
  FROM pg_stat_activity WHERE wait_event_type = 'Lock'
)
SELECT * FROM waiting WHERE cardinality(bloqueurs) > 0
ORDER BY age_transaction DESC NULLS LAST LIMIT 20;
```

### Transactions ouvertes inactives
```sql
SELECT pid, datname, now() - xact_start AS age_transaction
FROM pg_stat_activity WHERE state = 'idle in transaction'
ORDER BY xact_start NULLS LAST LIMIT 20;
```

### Lecture et action
- `bloqueurs` : PID à **vérifier**, pas à interrompre automatiquement ; `0` peut représenter une transaction préparée.
- Confirmer application, propriétaire, transaction et impact. Ne pas interroger les verrous en boucle.
- **Autorisation explicite requise :** envisager `pg_cancel_backend(PID_VALIDÉ)` pour annuler une requête ; `pg_terminate_backend` ferme la session (impact plus fort).
- **Validation :** chaîne de blocages disparue et application rétablie. Une annulation exécutée n'est pas réversible.

**Escalade N3 :** bloqueur critique, transaction préparée ou interruption risquée.

[Sources : statistiques](https://www.postgresql.org/docs/18/monitoring-stats.html) · [signaux backend](https://www.postgresql.org/docs/18/functions-admin.html)
