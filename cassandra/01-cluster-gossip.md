# CASS-N2-01 — Cluster, gossip & schéma

**Quand ?** Nœud `DN`, divergence de vues ou état du ring anormal.

### Vérifier depuis un nœud autorisé
```bash
nodetool -h HOST status
nodetool -h HOST describecluster
nodetool -h HOST gossipinfo
```

### Lecture et action
- `UN` = Up/Normal ; `DN` = Down/Normal ; `UJ`, `UL`, `UM` = mouvement de topologie.
- `status` reflète la **vue du nœud interrogé** : croiser avec d'autres nœuds/DC.
- `describecluster` indique notamment les versions de schéma visibles ; divergence à contextualiser avec un déploiement.
- `gossipinfo` peut révéler des adresses et états internes : expurger les sorties.
- **Action N2 :** corréler réseau, état du service, gossip et opérations planifiées ; correction uniquement après validation.
- **Validation :** nœuds attendus `UN`, vues cohérentes, erreurs et impact résolus.

**Escalade N3 :** incohérence de schéma durable, retrait de nœud, remplacement ou changement de topologie. Aucun `removenode` ou `decommission` réflexe.

[Sources : status](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/status.html) · [gossipinfo](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/gossipinfo.html)
