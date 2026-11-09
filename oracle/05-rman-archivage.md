# ORA-N2-05 — RMAN & archivage

**Quand ?** Backup échoué, destination archive en erreur, RPO menacé.

### Derniers jobs RMAN
```sql
SELECT input_type, status, start_time, end_time
FROM v$rman_backup_job_details
ORDER BY start_time DESC FETCH FIRST 15 ROWS ONLY;
```

### Destinations d'archivage (instance courante)
```sql
SELECT dest_id, status, error FROM v$archive_dest
WHERE status <> 'INACTIVE' ORDER BY dest_id;
```

### Lecture et action
- `COMPLETED` ne prouve pas la restaurabilité ; vérifier la chaîne et les archives dans l'outil RMAN/recovery catalog.
- `ERROR`, `FULL`, `BAD PARAM` : comparer aux destinations actives et attendues. Une destination `DEFERRED` peut être planifiée.
- L'historique local peut être incomplet ; comparer ordonnanceur, rétention et stockage externe.
- **Action N2 :** corréler erreur et capacité ; relance ou correction uniquement via la procédure approuvée, avec impact/RPO évalués.
- **Validation :** prochains jobs réussis et test de restauration isolé selon politique.

**Escalade N3 :** archives indispensables manquantes, PITR compromis ou restauration de production. Aucune suppression d'archivelogs à la main.

[Sources : RMAN](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-RMAN_BACKUP_JOB_DETAILS.html) · [V$ARCHIVE_DEST](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-ARCHIVE_DEST.html)
