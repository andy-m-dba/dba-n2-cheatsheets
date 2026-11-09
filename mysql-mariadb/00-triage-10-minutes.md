# MYSQL-N2-00 — Triage approfondi (10 min)

**Quand ?** Incident MySQL/MariaDB persistant après N1.

### 1. Cadrer (2 min)
Heure/fuseau, instance, version, impact, changements récents et mesures N1.

### 2. Vérifier l'instance (3 min)
```sql
SELECT VERSION() AS version, @@hostname AS hote,
       @@read_only AS lecture_seule, @@uptime AS uptime_s;
SHOW GLOBAL STATUS WHERE Variable_name IN
  ('Threads_connected','Threads_running','Aborted_connects');
```

### 3. Qualifier (3 min)
```sql
SHOW GLOBAL VARIABLES LIKE 'max_connections';
SHOW GLOBAL STATUS LIKE 'Max_used_connections';
```
Ces compteurs ne prouvent pas à eux seuls une panne. Comparer les relevés et la supervision.

### 4. Orienter (2 min)
[Verrous](01-verrous-transactions.md) · [SQL lent](02-performance-sql.md) · [InnoDB](03-innodb-io.md) · [Stockage](04-capacite-temporaires.md) · [Réplication](05-replication.md) · [Backups](06-backups-binlogs.md) · [Connexions](07-connexions-erreurs.md).

**Action N2 :** confirmer, évaluer les risques, obtenir l'autorisation, appliquer la procédure approuvée, valider côté application.

**Escalade N3 :** risque de perte de données, indisponibilité majeure ou cause incertaine.

[Source : MySQL SHOW STATUS](https://dev.mysql.com/doc/refman/8.4/en/show-status.html)
