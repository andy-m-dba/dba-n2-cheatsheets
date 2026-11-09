# ORA-N2-04 — Undo & ORA-01555

**Quand ?** `ORA-01555`, `ORA-30036` ou pression sur l'undo.

### Historique récent (par tranches de 10 min)
```sql
SELECT begin_time, end_time, ssolderrcnt,
       nospaceerrcnt, unxpstealcnt, tuned_undoretention
FROM v$undostat
ORDER BY begin_time DESC FETCH FIRST 12 ROWS ONLY;
```

### Paramètres en cours
```sql
SELECT name, value FROM v$parameter
WHERE name IN ('undo_management','undo_tablespace','undo_retention');
```

### Lecture et action
- `SSOLDERRCNT` : occurrences ORA-01555 **dans la tranche**, pas compteur global.
- `NOSPACEERRCNT` : manque d'espace undo ; `UNXPSTEALCNT` : tentative de réutilisation d'extents non expirés.
- `TUNED_UNDORETENTION` est en secondes ; sur Active Data Guard standby, cette valeur n'est pas représentative.
- **Action N2 :** analyser durée des requêtes, charge, espace et rétention ; changement de paramètre/datafile seulement après validation et plan de retour.
- **Validation :** erreurs disparues sur plusieurs intervalles, comportement applicatif confirmé.

**Escalade N3 :** espace undo critique, transactions longues ou changement structurel risqué.

[Source : V$UNDOSTAT](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-UNDOSTAT.html)
