# MDB-N2-07 — Logs & erreurs d'authentification

**Quand ?** `Unauthorized`, timeout répété, erreur intermittente.

### Identité de la session (droits autorisés)
```javascript
db.adminCommand({ connectionStatus: 1 }).authInfo.authenticatedUserRoles
```

### Événements récents du nœud
```javascript
db.adminCommand({ getLog: "global" }).log.slice(-20)
```

### Lecture et action
- `connectionStatus` décrit **la session courante**, pas tous les clients.
- `getLog` lit jusqu'aux **1024 derniers événements en cache RAM**, pas le fichier log complet.
- Un code `Unauthorized` peut relever des droits ou de la mauvaise cible ; distinguer auth et indisponibilité.
- **Action N2 :** corréler horodatage, application, TLS/ACL et supervision ; changement de rôle/certificat seulement après accord.
- **Validation :** nouvelles connexions autorisées et disparition des erreurs sur une période comparable.
- Ne publier ni secrets, ni URI complètes, ni messages ou commandes bruts.

**Escalade N3 :** changement d'authentification à grande portée, certificat critique ou incident persistant.

[Sources : connectionStatus](https://www.mongodb.com/docs/manual/reference/command/connectionstatus/) · [getLog](https://www.mongodb.com/docs/manual/reference/command/getlog/)
