# PG-N2-06 — Backups & archivage

**Quand ?** Archivage échoué, backup manquant, continuité PITR incertaine.

### Archivage PostgreSQL
```sql
SHOW archive_mode;
SELECT archived_count, failed_count, last_archived_wal,
       last_archived_time, last_failed_time, stats_reset
FROM pg_stat_archiver;
```

### Vérifier la chaîne
Dans l'outil réel (pgBackRest, Barman, snapshots ou managé) : dernier full/diff/incr, archivage requis, rétention, stockage distant, dernier test de restauration.

### Lecture et action
- `failed_count` : cumul depuis `stats_reset`, parfois incomplet.
- Dernier WAL archivé ≠ tous les WAL antérieurs archivés.
- Job `success` ≠ sauvegarde restaurable ni RPO respecté.
- **Action N2 :** corréler erreur, destination, capacité, ordonnanceur ; n'exécuter que la correction validée.
- **Validation :** prochains jobs réussis et test de restauration en environnement isolé, selon procédure.

**Escalade N3 :** PITR compromis, WAL manquant ou reprise risquée. Aucune purge WAL ou restauration en production sans plan approuvé.

[Sources : archivage](https://www.postgresql.org/docs/18/monitoring-stats.html) · [PITR](https://www.postgresql.org/docs/18/continuous-archiving.html)
