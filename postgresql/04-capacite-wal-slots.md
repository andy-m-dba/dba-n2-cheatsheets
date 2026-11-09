# PG-N2-04 — Capacité, WAL & slots

**Quand ?** Volume saturé, WAL en croissance, slot inactif.

### Stockage OS autorisé
```bash
df -h
df -i
```

### Slots sur le primaire
```sql
SELECT slot_name, slot_type, active, restart_lsn,
       pg_wal_lsn_diff(pg_current_wal_lsn(),restart_lsn) AS octets_lsn
FROM pg_replication_slots
WHERE restart_lsn IS NOT NULL ORDER BY octets_lsn DESC;
```

### Lecture et action
- `octets_lsn` = écart de positions WAL, **pas** une mesure directe du disque occupé.
- `active = false` peut être temporairement normal ; vérifier consommateurs, réplication et CDC.
- Corréler avec `df`, les volumes WAL, données, logs, tablespaces et backup.
- **Action N2 :** diagnostiquer le consommateur et demander une correction approuvée. Ne jamais effacer des WAL ni supprimer un slot sans analyse de perte de données et plan de reprise.
- **Validation :** baisse de l'occupation et continuité de réplication/CDC.

**Escalade N3 :** saturation imminente, slot inconnu, WAL indispensable manquant.

[Sources : slots](https://www.postgresql.org/docs/18/view-pg-replication-slots.html) · [WAL](https://www.postgresql.org/docs/18/wal-configuration.html)
