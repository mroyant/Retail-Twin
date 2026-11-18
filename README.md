# Retail-Twin

Un projet d'analyse comportementale par vision par ordinateur en supermarché : il transforme un parc de caméras de sécurité existant en un capteur d'aide à la décision opérationnelle et de collecte de data.

**En une phrase :** on suit des silhouettes anonymes sur les flux vidéo existants, on les projette sur le plan du magasin, et on en tire des indicateurs qui servent à prendre des décisions concrètes. Par exemple : ouvrir une caisse maintenant, placer un employé au rayon coupe à 11h30, remplir l'allée 7 à 14h plutôt qu'à 17h.

## Organisation du repo

| Dossier | Contenu | Pour qui |
|---|---|---|
| [docs/](docs/) | Vision, analyse du besoin, **cas d'usage**, architecture, modèle de données, RGPD, roadmap, décisions (ADR) | Toute l'équipe |
| [poc/](poc/) | Proof of concept Python : vidéo → tracking → coordonnées plan → indicateurs par zone | Dev / data |
| [presentation/](presentation/) | Trame du pitch, scénario de démo, visuels pour la présentation finale | Présentation produit |
| [.github/](.github/) | Templates d'issues et de PR | Collaboration |

## Par où commencer

1. Lire [docs/01-vision.md](docs/01-vision.md) puis [docs/02-cas-usage.md](docs/02-cas-usage.md) (analyse du besoin et cas d'usage priorisés).
2. Lire [docs/05-rgpd-conformite.md](docs/05-rgpd-conformite.md) : c'est un point bloquant pour un déploiement réel.
3. Voir le périmètre et la structure prévue du POC : [poc/README.md](poc/README.md).
4. Avant de contribuer, lire [CONTRIBUTING.md](CONTRIBUTING.md) (branches, commits, PR).

## Statut

Phase **cadrage** : la doc et la structure sont en place, le code du POC n'est pas encore démarré. Voir [docs/06-roadmap.md](docs/06-roadmap.md).
