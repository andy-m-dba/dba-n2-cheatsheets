# CASS-N2-07 — Clients CQL, timeouts & logs

**Quand ?** Connexion applicative échouée, timeouts et erreurs sans cause confirmée.

### Nœud ciblé (JMX autorisé)
```bash
nodetool -h HOST clientstats
nodetool -h HOST statusbinary
```

### Vérifier CQL (hôte autorisé)
```bash
cqlsh HOST 9042
```
Port, `--ssl` et profil d'authentification : suivre l'inventaire. Ne jamais placer de mot de passe dans la commande.

```sql
SELECT cluster_name, release_version, data_center FROM system.local;
```

### Lecture et action
- `clientstats` : connexions natives du nœud, **pas** tous les clients du cluster. Sorties sensibles.
- `statusbinary` : transport natif actif/inactif ; un service actif ne prouve pas la connectivité de l'application.
- `cqlsh` teste **un seul nœud** ; comparer TLS, driver, réseau, DC et cohérence avec les logs autorisés.
- **Action N2 :** qualifier la rupture et faire approuver la correction côté driver, accès ou service.
- **Validation :** tests CQL et applicatifs réussis, erreurs et latence stabilisées.

**Escalade N3 :** divergence de schéma, problème de driver/consistance complexe ou incident multi-DC.

[Sources : clientstats](https://cassandra.apache.org/doc/latest/cassandra/managing/tools/nodetool/clientstats.html) · [cqlsh](https://cassandra.apache.org/doc/stable/cassandra/managing/tools/cqlsh.html)
