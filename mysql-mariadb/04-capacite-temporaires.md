# MYSQL-N2-04 — Capacité & tables temporaires

**Quand ?** Volume saturé, croissance de fichiers ou temporaires disque.

### Taille estimée des bases
```sql
SELECT TABLE_SCHEMA AS base,
       ROUND(SUM(DATA_LENGTH+INDEX_LENGTH)/1048576,1) AS taille_mio
FROM information_schema.TABLES
WHERE TABLE_SCHEMA NOT IN ('mysql','sys','information_schema','performance_schema')
GROUP BY TABLE_SCHEMA ORDER BY taille_mio DESC;
```

### Temporaires et destination
```sql
SHOW GLOBAL STATUS WHERE Variable_name IN
 ('Created_tmp_tables','Created_tmp_disk_tables');
SHOW GLOBAL VARIABLES LIKE 'tmpdir';
```

### Volumes (hôte autorisé)
```bash
df -h
df -i
```

### Lecture et action
- `DATA_LENGTH` / `INDEX_LENGTH` sont **estimés** pour InnoDB ; binlogs, redo et autres fichiers sont hors total.
- Compteurs temporaires **cumulatifs** et non exhaustifs selon version/mécanisme ; comparer deux relevés.
- **Action N2 :** confirmer le volume concerné et la cause avant extension ou réglage approuvé.
- **Validation :** espace libre et évolution de la croissance, sans dégradation applicative.

**Escalade N3 :** saturation imminente, purge de journaux envisagée ou corruption.

[Source : INFORMATION_SCHEMA.TABLES](https://dev.mysql.com/doc/refman/8.4/en/information-schema-tables-table.html)
