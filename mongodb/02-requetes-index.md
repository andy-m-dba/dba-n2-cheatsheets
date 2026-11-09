# MDB-N2-02 — Requêtes lentes & index

**Quand ?** Régression de latence ou plan d'accès suspect.

### Index de la collection ciblée
```javascript
const c = db.getSiblingDB("DATABASE").getCollection("COLLECTION")
c.getIndexes().map(i => ({ nom: i.name, cles: i.key, masque: i.hidden }))
```

### Plan estimé d'une requête ciblée
```javascript
c.find({ CHAMP: "VALEUR_TEST" }).explain("queryPlanner")
```
Remplacer collection, champ et filtre par un **cas autorisé**, sans afficher de documents.

### Lecture et action
- Examiner `winningPlan`, `IXSCAN` / `COLLSCAN` et les champs filtrés.
- Un `COLLSCAN` n'est pas automatiquement anormal : tenir compte de la taille et de la sélectivité.
- `queryPlanner` n'exécute **pas** la recherche ; `executionStats` l'exécute et peut générer de la charge.
- **Action N2 :** proposer index ou réécriture après mesure, revue d'impact, test et plan de retour.
- **Validation :** comparer plan, latence et consommation réelle sur une charge similaire.

**Escalade N3 :** changement d'index critique, plan instable ou architecture de requêtes à revoir.

[Sources : explain](https://www.mongodb.com/docs/manual/reference/command/explain/) · [getIndexes](https://www.mongodb.com/docs/manual/reference/method/db.collection.getindexes/)
