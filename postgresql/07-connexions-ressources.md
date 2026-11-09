# PG-N2-07 — Connexions & ressources

**Quand ?** `too many connections`, pool saturé, erreurs applicatives.

### Sessions par base et état
```sql
SELECT datname, state, count(*) AS sessions
FROM pg_stat_activity WHERE backend_type = 'client backend'
GROUP BY datname, state ORDER BY sessions DESC;
```

### Paramètres chargés
```sql
SELECT name, setting, unit, source, pending_restart
FROM pg_settings
WHERE name IN ('max_connections', 'superuser_reserved_connections',
               'reserved_connections', 'work_mem', 'maintenance_work_mem')
ORDER BY name;
```

### Lecture et action
- `reserved_connections` existe depuis PostgreSQL 16 : absence normale en 14/15.
- `max_connections` comporte des emplacements réservés ; vérifier pools et tendances.
- `pending_restart = true` signifie redémarrage nécessaire pour appliquer un paramètre, **pas** autorisation de redémarrer.
- **Action N2 :** identifier une fuite ou un pool mal dimensionné, proposer correction avec impact, validation et plan de retour ; ne pas augmenter aveuglément `max_connections`.
- **Validation :** erreurs et connexions stabilisées, confirmation applicative.

**Escalade N3 :** saturation persistante ou changement d'architecture nécessaire.

[Source : configuration des connexions](https://www.postgresql.org/docs/18/runtime-config-connection.html)
