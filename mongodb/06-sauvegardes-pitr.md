# MDB-N2-06 — Backups & Point-in-Time Recovery

**Quand ?** Snapshot manquant, sauvegarde en échec, objectif RPO menacé.

### Vérifier dans l'outil officiel (lecture seule)
- **Atlas :** cluster → Backup / Snapshots ; vérifier politique, dernier succès, rétention et PITR disponible selon l'offre.
- **Self-managed :** Ops Manager, ordonnanceur ou solution validée ; vérifier les nœuds/shards et l'emplacement des sauvegardes.
- **Oplog :** confirmer que la solution conserve les journaux nécessaires à la période de restauration demandée.

### Lecture et action
- Backup `Completed` ≠ restauration réellement réussie ; vérifier les exercices de restauration.
- Un `mongodump` seul n'offre pas automatiquement une restauration à n'importe quel instant.
- Sur cluster sharded, la **cohérence inter-shards** et les métadonnées doivent être prises en compte.
- **Action N2 :** qualifier la chaîne, l'échec et le RPO ; relance **uniquement** via la procédure approuvée.
- **Validation :** nouveau job réussi et restauration isolée testée, selon politique.

**Escalade N3 :** PITR impossible, métadonnées manquantes ou restauration de production nécessaire. Aucune restauration ni purge autonome.

[Sources : Atlas Backup](https://www.mongodb.com/docs/atlas/backup/cloud-backup/overview/) · [Self-managed](https://www.mongodb.com/docs/manual/core/backups/)
