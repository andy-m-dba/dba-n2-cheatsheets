# DBA N2 — CheatSheets

**But :** approfondir un incident DBA identifié au N1, choisir une action encadrée et vérifier le résultat. **Un problème = une fiche courte.**

**Règle N2 :** diagnostiquer → établir les faits → mesurer le risque → obtenir l'autorisation nécessaire → agir → valider / escalader.

## PostgreSQL

| Situation | Fiche |
|---|---|
| Incident non résolu par N1 | [00 — Triage N2 (10 min)](postgresql/00-triage-10-minutes.md) |
| Blocages et longues transactions | [01 — Verrous et transactions](postgresql/01-verrous-transactions.md) |
| Requêtes lentes | [02 — Performance SQL](postgresql/02-performance-sql.md) |
| Tables et autovacuum | [03 — Vacuum & statistiques](postgresql/03-vacuum-statistiques.md) |
| Saturation disque / WAL | [04 — Capacité & slots](postgresql/04-capacite-wal-slots.md) |
| Retard ou panne de standby | [05 — Réplication](postgresql/05-replication.md) |
| Échec des backups / archivage | [06 — Sauvegardes & archivage](postgresql/06-sauvegardes-archivage.md) |
| Connexions et ressources | [07 — Connexions & ressources](postgresql/07-connexions-ressources.md) |

**Versions visées :** PostgreSQL 14–18. PostgreSQL 14 arrive en fin de support le **12/11/2026**. Vérifier versions, architecture, droits et procédures d'exploitation avant intervention.

**Utilisation :** toutes les commandes de diagnostic sont en lecture seule. Les actions correctives éventuelles nécessitent validation du périmètre, autorisation et analyse d'impact. Ne jamais publier de secrets, logs bruts ou données métier.

## SQL Server

| Situation | Fiche |
|---|---|
| Incident non résolu par N1 | [00 — Triage N2 (10 min)](sqlserver/00-triage-10-minutes.md) |
| Blocages / transactions | [01 — Blocages](sqlserver/01-blocages-transactions.md) |
| Requêtes lentes / plans | [02 — Query Store](sqlserver/02-performance-query-store.md) |
| Attentes / stockage lent | [03 — Attentes & I/O](sqlserver/03-attentes-io.md) |
| Saturation tempdb / LDF | [04 — tempdb & journal](sqlserver/04-tempdb-journal.md) |
| Index / statistiques | [05 — Index & stats](sqlserver/05-index-statistiques.md) |
| Sauvegardes / RPO | [06 — Sauvegardes](sqlserver/06-sauvegardes.md) |
| Réplication / disponibilité | [07 — Always On](sqlserver/07-always-on.md) |

**Versions visées :** SQL Server 2019, 2022 et 2025 (instances classiques, hors Azure SQL Database). Les DMV de supervision demandent généralement `VIEW SERVER STATE` en 2019 et `VIEW SERVER PERFORMANCE STATE` en 2022+. Se référer aux droits exacts de chaque vue et à la politique de moindre privilège.

**Données sensibles :** ne pas publier le texte des requêtes métier, noms internes ou sorties brutes.

## Oracle

| Situation | Fiche |
|---|---|
| Incident persistant | [00 — Triage N2 (10 min)](oracle/00-triage-10-minutes.md) |
| Verrous et timeouts | [01 — Sessions & verrous](oracle/01-sessions-verrous.md) |
| SQL lent / charge | [02 — Performance SQL](oracle/02-performance-sql.md) |
| Espace, ORA-01653, FRA | [03 — Tablespaces & FRA](oracle/03-tablespaces-fra.md) |
| ORA-01555 / undo | [04 — Undo](oracle/04-undo.md) |
| Backup / archivelog | [05 — RMAN & archivage](oracle/05-rman-archivage.md) |
| Retard de standby | [06 — Data Guard](oracle/06-data-guard.md) |
| PDB indisponible | [07 — CDB / PDB](oracle/07-cdb-pdb.md) |

**Versions visées :** Oracle Database 19c (CDB/PDB ; RAC et Data Guard selon architecture). Pour les autres versions, vérifier la compatibilité des vues et des droits. Les commandes sont en lecture seule. **AWR/ASH/ADDM exigent Diagnostics Pack** : licence à vérifier avant usage.

**Statut :** fiches documentaires, non exécutées sur une instance réelle.

Documentation : [PostgreSQL](https://www.postgresql.org/docs/current/) · [SQL Server](https://learn.microsoft.com/en-us/sql/sql-server/) · [Oracle Database](https://docs.oracle.com/en/database/oracle/oracle-database/19/)
