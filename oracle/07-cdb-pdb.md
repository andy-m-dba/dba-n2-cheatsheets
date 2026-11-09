# ORA-N2-07 — CDB / PDB indisponible

**Quand ?** Service métier inaccessible alors que l'instance répond.

### Conteneur de connexion
```sql
SELECT SYS_CONTEXT('USERENV','CON_NAME') AS conteneur FROM dual;
```

### PDB visibles
```sql
SELECT con_id, name, open_mode, restricted
FROM v$pdbs ORDER BY con_id;
```

### Lecture et action
- Depuis `CDB$ROOT`, vue des PDB de l'instance ; depuis une PDB, visibilité limitée à celle-ci.
- `MOUNTED` : PDB non ouverte aux connexions usuelles ; `READ ONLY` peut être attendu.
- `RESTRICTED=YES` limite les connexions ; `PDB$SEED` en lecture seule est normal.
- **Action N2 :** confirmer PDB/service, maintenance et rôle Data Guard ; ouverture ou modification de service uniquement sous procédure approuvée et avec plan de retour.
- **Validation :** mode attendu, service joignable, connexion applicative et logs sans nouvelle anomalie.

**Escalade N3 :** échec d'ouverture, recovery PDB, erreur de fichiers ou modification multitenant risquée.

[Sources : V$PDBS](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-PDBS.html) · [Monitoring PDB](https://docs.oracle.com/en/database/oracle/oracle-database/19/multi/viewing-information-about-cdbs-and-pdbs-with-sql-plus.html)
