# ORA-N2-03 — Tablespaces & FRA

**Quand ?** ORA-01653, ORA-19809, espace presque saturé.

### Tablespaces (droits dictionnaire requis)
```sql
SELECT tablespace_name, ROUND(used_percent,1) AS pct_utilise
FROM dba_tablespace_usage_metrics
ORDER BY used_percent DESC;
```

### Fast Recovery Area
```sql
SELECT ROUND(space_limit/1048576) AS limite_mio,
       ROUND(space_used/1048576) AS utilise_mio,
       ROUND(space_reclaimable/1048576) AS recuperable_mio
FROM v$recovery_file_dest;
```

### Lecture et action
- `USED_PERCENT` tient compte de la capacité de croissance possible, pas uniquement des fichiers alloués.
- `SPACE_USED` peut inclure d'anciennes FRA ; **ce n'est pas** l'occupation réelle du volume OS/ASM.
- `SPACE_RECLAIMABLE` ne donne aucune autorisation de purge.
- **Action N2 :** comparer supervision OS/ASM, autoextend, archive et rétention ; extension ou correction uniquement après validation de capacité et impact.
- **Validation :** seuils revenus à la normale, archiver fonctionnel et service stable.

**Escalade N3 :** saturation imminente ou suppression d'archives/backup envisagée. Ne jamais effacer de fichiers à la main.

[Sources : tablespaces](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/DBA_TABLESPACE_USAGE_METRICS.html) · [FRA](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-RECOVERY_FILE_DEST.html)
