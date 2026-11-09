# MYSQL-N2-07 — Connexions & erreurs

**Quand ?** `Too many connections`, authentifications rejetées, pool instable.

### Compteurs & capacité
```sql
SHOW GLOBAL STATUS WHERE Variable_name IN
 ('Threads_connected','Threads_running','Max_used_connections',
  'Aborted_connects','Uptime');
SHOW GLOBAL VARIABLES LIKE 'max_connections';
```

### Paramètre de timeout
```sql
SHOW GLOBAL VARIABLES LIKE 'wait_timeout';
```

### Lecture et action
- `Threads_connected` : connexions ouvertes ; `Threads_running` : threads actifs.
- `Max_used_connections` : pic observé depuis la dernière réinitialisation des statistiques.
- `Aborted_connects` : échecs de connexion **cumulatifs**, pas forcément mots de passe erronés.
- Un compteur élevé ne prouve ni fuite de connexions ni surcharge : confronter à l'application et à plusieurs relevés.
- **Action N2 :** identifier pool, erreurs et limites ; changer `max_connections` ou timeouts seulement après étude d'impact et rollback prévu.
- **Validation :** erreurs et connexions stabilisées, test applicatif réussi.

**Escalade N3 :** saturation persistante, incident TLS/auth complexe ou changement d'architecture.

[Sources : MySQL](https://dev.mysql.com/doc/refman/8.4/en/server-status-variables.html) · [MariaDB](https://mariadb.com/docs/server/reference/sql-statements/administrative-sql-statements/show/show-status)
