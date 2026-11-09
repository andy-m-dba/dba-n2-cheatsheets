# REDIS-N2-06 — ACL & authentification

**Quand ?** `NOAUTH`, `WRONGPASS`, `NOPERM` ou accès applicatif refusé.

### Identifier le compte de la session autorisée
```text
ACL WHOAMI
```

### Événements de sécurité récents, si autorisé
```text
ACL LOG 5
```

### Lecture et action
- `WHOAMI` retourne le nom de l'utilisateur **de la connexion actuelle**, pas celui de l'application en panne.
- `ACL LOG` distingue `auth`, `command`, `key`, `channel` ; la sortie expose utilisateurs, adresses ou ressources : **expurger**.
- Redis Software/Cloud peut limiter les commandes ACL ; utiliser la console d'administration prévue.
- **Action N2 :** confirmer compte, cible, TLS et autorisations attendues ; modification ACL uniquement après validation de sécurité et rollback.
- **Validation :** test de connexion par le compte applicatif autorisé, sans élargir les permissions inutilement.

**Escalade N3 :** rotation de secrets critique, changement global d'ACL ou incident de sécurité.

[Sources : ACL WHOAMI](https://redis.io/docs/latest/commands/acl-whoami/) · [ACL LOG](https://redis.io/docs/latest/commands/acl-log/)
