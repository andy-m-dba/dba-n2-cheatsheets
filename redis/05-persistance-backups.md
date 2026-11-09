# REDIS-N2-05 — RDB, AOF & sauvegardes

**Quand ?** `MISCONF`, sauvegarde RDB échouée, erreur AOF ou RPO menacé.

### État de la persistance
```text
INFO persistence
```

### Vérification du stockage (hôte autorisé)
```bash
df -h
df -i
```

### Lecture et action
- `rdb_last_bgsave_status:err` : dernier snapshot RDB échoué ; examiner logs, disque, permissions et mémoire.
- `aof_enabled:1` : AOF actif ; `aof_last_write_status:err` : dernière écriture AOF échouée.
- `rdb_bgsave_in_progress` / `aof_rewrite_in_progress` : opérations en cours, pas preuve de sauvegarde externe.
- **Action N2 :** corriger la cause seulement selon procédure et risques établis ; ne pas désactiver une protection d'écriture pour masquer `MISCONF`.
- **Validation :** prochains états RDB/AOF corrects, backup externe et restauration testée selon politique.

**Escalade N3 :** AOF/RDB corrompu, données manquantes ou restauration en production nécessaire. Aucune purge manuelle.

[Sources : INFO](https://redis.io/docs/latest/commands/info/) · [MISCONF](https://redis.io/docs/latest/develop/reference/errors/common/misconf/)
