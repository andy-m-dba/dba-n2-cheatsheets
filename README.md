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

**Statut :** fiches documentaires, non exécutées sur une instance réelle.

Documentation : [PostgreSQL](https://www.postgresql.org/docs/current/) · [Versions supportées](https://www.postgresql.org/support/versioning/)
