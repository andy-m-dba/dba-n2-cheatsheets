# CASS-N2-06 — Sauvegardes & snapshots

**Quand ?** Backup échoué, snapshot incomplet, RPO menacé.

### Contrôles par nœud (JMX autorisé)
```bash
nodetool -h HOST statusbackup
nodetool -h HOST listsnapshots
```

### Lecture et action
- `statusbackup` : activation du backup incrémental, **pas** la réussite d'une sauvegarde.
- `listsnapshots` : snapshots **locaux** avec tailles ; ne garantit ni export ni couverture de tous les nœuds.
- Les liens physiques des SSTables peuvent rendre trompeuse l'addition simple des tailles de snapshots.
- Vérifier dans l'outil officiel : dernier succès, couverture des DC/nœuds, stockage externe et rétention.
- **Action N2 :** corréler le job, les erreurs et la capacité ; relance uniquement via procédure approuvée.
- **Validation :** sauvegarde complète et restauration **isolée** testée selon politique.

**Escalade N3 :** restauration de production, données manquantes ou cohérence multi-nœuds non garantie. Aucun `clearsnapshot` réflexe.

[Sources : statusbackup](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/statusbackup.html) · [listsnapshots](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/listsnapshots.html)
