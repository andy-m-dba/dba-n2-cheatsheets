# CASS-N2-05 — Cohérence, repair & streaming

**Quand ?** Réplica en retard, données potentiellement divergentes, réparation échouée.

### Activité réseau et réparation (JMX autorisé)
```bash
nodetool -h HOST netstats
nodetool -h HOST tpstats
nodetool -h HOST status
```

### Contrôles hors nœud
Vérifier les jobs et logs de repair de l'ordonnanceur, la dernière réussite **par DC, nœud et table**, et les durées d'indisponibilité.

### Lecture et action
- `netstats` : streaming en cours depuis ce nœud ; une vue vide n'atteste pas la cohérence.
- Hinted handoff ≠ réparation complète ; des écritures manquées peuvent subsister.
- Un repair concerne les plages de tokens du nœud exécutant, **pas** automatiquement tout le cluster.
- `repair --preview` ne transfère pas de données, mais peut consommer des ressources : **aucun lancement sans autorisation**.
- **Action N2 :** qualifier portée, fenêtre, réseau et capacité ; repair uniquement selon procédure et charge validées.
- **Validation :** jobs et streaming cohérents, erreurs disparues et état applicatif confirmé.

**Escalade N3 :** repair complet, divergence, risque lié aux tombstones ou nœud à reconstruire.

[Sources : repair](https://cassandra.apache.org/doc/stable/cassandra/managing/operating/repair.html) · [netstats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/netstats.html)
