# MSSQL-N2-05 — Index & statistiques

**Quand ?** Régression de plan, cardinalités suspectes, lenteurs ciblées.

### Statistiques de la base courante
```sql
SELECT TOP (15) OBJECT_SCHEMA_NAME(s.object_id) AS schema_name,
       OBJECT_NAME(s.object_id) AS table_name, s.name AS statistique,
       p.last_updated, p.modification_counter
FROM sys.stats AS s
CROSS APPLY sys.dm_db_stats_properties(s.object_id,s.stats_id) AS p
WHERE OBJECTPROPERTY(s.object_id,'IsUserTable')=1
ORDER BY p.modification_counter DESC;
```

### Index d'une table ciblée seulement
```sql
DECLARE @objet int = OBJECT_ID(N'dbo.TABLE_CIBLE',N'U');
IF @objet IS NOT NULL
  SELECT index_id, page_count, avg_fragmentation_in_percent
  FROM sys.dm_db_index_physical_stats(DB_ID(),@objet,NULL,NULL,'LIMITED')
  WHERE page_count >= 1000;
```

### Lecture et action
- `modification_counter` seul ne justifie pas une mise à jour ; valider plan et distribution.
- Fragmentation isolée ≠ problème ; le mode `LIMITED` reste une lecture à cibler.
- **Action N2 :** statistiques/index uniquement selon fenêtre et procédure approuvées (coût I/O, verrous).
- **Validation :** plan, durée et charge réelles avant/après.

**Escalade N3 :** restructuration d'index ou schéma à fort impact.

[Sources : stats](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql) · [index](https://learn.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql)
