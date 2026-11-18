# Contribuer à Retail-Twin

## Organisation du travail

- **Tâches** : une issue GitHub par tâche, avec un template (cas d'usage, tâche, bug). On s'assigne l'issue avant de commencer pour éviter de faire le travail en double.
- **Suivi** : un board GitHub Projects avec les colonnes *À faire / En cours / En revue / Fait*.
- **Décisions** : toute décision structurante fait l'objet d'un ADR dans [docs/adr/](docs/adr/).

## Branches

- `main` est protégée : on n'y pousse pas directement, tout passe par une PR.
- On crée une branche par issue :
  - `feat/<num-issue>-description-courte` : nouvelle fonctionnalité
  - `fix/<num-issue>-...` : correction
  - `docs/<num-issue>-...` : documentation
  - `exp/<nom>` : expérimentation jetable (notebooks), qui n'a pas vocation à être fusionnée telle quelle

## Commits

Format [Conventional Commits](https://www.conventionalcommits.org/fr/), en français :

```
feat(analytics): calcul du dwell par zone
fix(geometry): homographie inversée sur cam_caisses
docs(cas-usage): ajout UC-12 rayon traiteur
```

## Pull requests

- Une PR = un sujet. Elle reste petite, pour être relue en moins de 15 minutes.
- Elle référence l'issue (`Closes #12`).
- Elle doit être **relue par l'autre personne** avant d'être fusionnée (*squash merge*).

## Règles données & vidéo (non négociables)

- **Jamais de vidéo, d'image de personne ou de donnée client dans git.** `poc/data/` est ignoré.
- Pas de clé, de mot de passe ou d'URL RTSP avec identifiants dans le code : utiliser un fichier `.env` (ignoré par git).
- Voir [docs/05-rgpd-conformite.md](docs/05-rgpd-conformite.md).

## Réglages GitHub à faire une fois (propriétaire du repo)

1. *Settings → Collaborators* : inviter le ou la collaborateur·rice.
2. *Settings → Branches* : protéger `main` (PR obligatoire, 1 approbation).
3. *Settings → General* : n'autoriser que le *squash merge*.
4. Créer le board *Projects* et les labels `P0`, `P1`, `P2`, `doc`, `poc`, `presentation`, `rgpd`.
